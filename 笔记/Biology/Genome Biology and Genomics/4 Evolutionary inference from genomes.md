# Variant calling

# Practice 
```bash
# BluePebble: 
module load apps/bwa/0.7.17
module load apps/samtools/1.9
module load apps/bcftools 
module load apps/snpeff/5.0c
```


```
cd /user/work/ms22568/GenomeBiologyandGenomics/session4
```

## Part 1
### BWA
BWA，即 Burrows-Wheeler-Alignment Tool。**BWA 软件的作用是将序列比对到参考基因组上**，在比对之前，**首先**需要对参考基因组建立索引。

To index the reference sequence using the bwa aligner:
```bash
bwa index covid19_wuhan_reference.fasta
```

三个不同的算法：
-   BWA-backtrack: 是用来比对 Illumina 的序列的，reads 长度最长能到 100bp。-
-  BWA-SW: 用于比对 long-read ，支持的长度为 70bp-1Mbp；同时支持剪接性比对。
-   **BWA-MEM:** 推荐使用的算法，支持较长的 read 长度，同时支持剪接性比对（split alignments)，但是 BWA-MEM 是更新的算法，也更快，更准确，且 BWA-MEM 对于 70bp-100bp 的 Illumina 数据来说，效果也更好些。

Map the reads from the first variant to the reference genome using bwa. 
```bash
bwa mem covid19_wuhan_reference.fasta variant1_R1.fastq variant1_R2.fastq -o variant1.sam
```
it writes the alignment of reads to the reference to a Sequence Alignment Map (SAM) format file, the standard for storing this kind of information in genomics.

### SAM to BAM
Convert the SAM file to a compressed BAM file, then sort it (needed for variant calling):
```bash
samtools view -S -b variant1.sam > variant1.bam
samtools sort variant1.bam > variant1_sorted.bam
```
+ -S input is SAM: 默认下输入是 BAM 文件，若是输入是 SAM 文件，则最好加该参数，否则有时候会报错
+ -b output BAM: 默认下输出是 SAM 格式文件，该参数设置输出 BAM 格式

bam 文件优点：bam 文件为二进制文件，占用的磁盘空间比 sam 文本文件小；利用 bam 二进制文件的运算速度快。

之后的sort 对 bam 文件进行排序。一些软件需要 sort 的 bam 或者 sam 文件，如 stringtie，所以必须要 sort 使用；求 depth 时，也必须要 sort；

Visualise/inspect the alignment of reads from the variant against the reference sequence using the samtools tview option.
```bash
# do index firstly
samtools index variant1_sorted.bam
samtools tview variant1_sorted.bam covid19_wuhan_reference.fasta
```
必须对 bam 文件进行默认情况下的排序后，才能进行 index。否则会报错。建立索引后将产生后缀为`.bai `的文件，用于快速的随机处理。很多情况下需要有 bai 文件的存在，特别是显示序列比对 (`tview`)情况下。

### Bcftools
We will use bcftools mpileup and bcftools call for variant calling. Note the use of the pipe (|) to pass information from one method to the next. 
```bash
bcftools mpileup -f covid19_wuhan_reference.fasta variant1_sorted.bam | bcftools call -mv -Ob --ploidy 1 -o calls.bcf 
```
参数1：
+ mpileup: 用 pileup 方法检测变异
+ -f: FASTA 格式的参考基因组文件，需提前建立索引
参数2：
+ call: 用于 SNP/INDEL 的检测，输入文件为 VCF/BCF 格式
+ -m: 优化后的变异检测参数
+ -v:  只输出有变异的位点
+ -O,--output-type b/u/z/v 规定输出文件的格式，b 表示压缩的 BCF 格式，u 表示未压缩的 BCF 格式，z 表示压缩的 VCF 格式，v 表示未压缩的 VCF 格式
+ --ploidy 1: 在 VCF/BCF 文件中指定样本的倍性，1表示样本是单倍体
+ -o: 输出文件，写入 FILE

Filter the list of variants to exclude those with low quality scores. 
```bash
bcftools view -i '%QUAL>=20' calls.bcf > calls.vcf
```
view 命令转换成 VCF 文件

### Predict the functional/coding-level consequences
snpEff:
```bash
java -Xmx2g -jar /sw/apps/snpeff-5.0c/snpEff/snpEff.jar -v NC_045512.2 calls.vcf > annotation.vcf
```
参数:
+ java -jar: Java环境下运行程序
+ -Xmx: 指定 Java 虚拟机(JVM)的最大内存分配池 （Xms 指定初始内存分配池）
+ -v: 设置在程序运行过程中输出的日志信息
+ NC_045512.2: Genome name

## Questions
How many high-quality SNPs do you detect?
> 33 (variant1)

What are the amino acid-level changes that are caused by these SNPs? 
> missense_variant	- 30 - 5.714%

In which genes do they occur?


## Part 2
Assemble the genomes of the two SARS-CoV-2 variants and determine their geographical origin using phylogenetics. This part of the practical is rendered fairly straightforward thanks to the Pangolin (“Phylogenetic Assessment of Named Global Outbreak Lineages”, https://pangolin.cog-uk.io/) infrastructure for COVID epidemiology/phylogenetic placement. 
