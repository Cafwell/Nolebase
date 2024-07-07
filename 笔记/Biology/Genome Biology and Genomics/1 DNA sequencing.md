# Why sequencing?

1. Characterise all genes (blue) and regulatory elements (red)
![[../../assets/genes and RE.png|550]]
2. Physically linked genes may form pathways 
	+ An intriguing feature of specialized metabolic pathways in fungi is that constituent genes are often physically linked on chromosomes forming what are known as gene clusters (Wisecaver, Jennifer H., Jason C. Slot, and Antonis Rokas. "The evolution of fungal metabolic pathways." _PLoS genetics_ 10.12 (2014): e1004816.)
3. Compare genes, finding mutations and so on
4. Identify candidate genes close to a genetic marker associated with trait

# Sequencing transcriptomes

The transcriptome is the **sum of all the RNAs** produced by a species or a specific cell in a functional state, including mRNA and Non-coding RNAs (Non-coding).

Align RNA reads to a reference and count expression levels:
![[../../assets/align.png]]

In gene X, Exon2 is not expressed 


# Starting materials 

## DNA
Near-identical DNA: Plant roots and shoots
Non-identical DNA: Wheat (hexaploid); Blood cells (Red & White)

PS: [关于多倍体](https://zhuanlan.zhihu.com/p/91689320)

## RNA
Tissue-specific expression: Plant roots and leaves
Time-specific expression: Plant leaves under high or low light

### 16S rRNA
原核生物有 16S、23S 及 5S 三种 rRNA，这三种 rRNA 均存在于 30S 的 rRNA 前体中。转录作用完成后，在 RNaseⅢ 催化下，将 rRNA 前体切开产生 16S、25S 及 5S rRNA 的中间前体。进一步在核酸酶的作用下，切去部分间隔序列，产生成熟的 16S、23S 及 5S rRNA，还有成熟的 tRNA。

![[../../assets/16S.jpeg|200]]

16S rRNA gene sequencing is an important means to study microbial community composition and distribution.

## An ideal sequence?
~~1)~~ Full length chromosomes/genome/transcripts    
~~2)~~ No error      
Both of them cannot be achieved.

Currently choose:    
~ 1Mb with ~5% error-rate    
~ 300 bp with 0.1% error-rate 

Physical limits of extracted DNA size may limit even if sequencing technology doesn’t 

-> Use bioinformatics and other techniques to make up for these shortcomings

# Sequencing technology

## First generation
**Sanger** sequencing:
+ Read-length *up to ∼1,000bp* 
+ Accuracy ∼99.99%

## Next generation sequencing, NGS
There are the second generation, the third generation and even the fourth generation

The common features of NGS are miniaturization, high-throughput parallelization and low cost. (微量化、高通量并行化和低成本)

### Second Generation 
**Illumina** sequencing – from 2008
+ Only *250-300bp in length
+ Error rate 1-2%

Illumina sequencing platforms:     
![[../../assets/Illumina_machines.png|400]]

Steps:   
1) Construction of DNA sequencing library 
![[../../assets/illumina_1.png|400]]

Adapters contain:
-   Sequences that allow the library to bind and generate clusters on the flow cell (p5 and p7 sequences)
-   Sequencing primer binding sites to initiate sequencing (Rd1 SP and Rd2 SP)
-   Index sequences (Index 1 and, where applicable, Index 2), which are sample identifiers that allow multiplexing/pooling of multiple samples in a single sequencing run or flow cell lane.
![[../../assets/Illumina adapter.png|400]]

2) Bridge PCR amplification and Cluster Generation
![[../../assets/Bridge amplification.png|400]]
![[../../assets/illumina_2.png|400]]
3) Sequencing     
![[../../assets/illumina_3.png|400]]

#### Single-Read/Paired-end/Mate-paired 
+ **Single-Read**: Sequencing a single-ended read sequence, only need one SP on one side when building the library 
+ **Paired-end**: Normal steps following above. Perform the synthetic sequencing of the complementary chains
	+ Both ends sequenced and then spliced by overlapping sections to achieve high-quality sequencing;
	+ Single-read will only have sequence information, it is difficult to determine the position of this repeated sequence and cause error or dislocation;    
![[../../assets/single-end_pair-end.png|300]]
+ **Mate-paired**: For longer sequences, cycling the ends with biotin, and then the cycled genes are typed into short sequences. The biotin-containing sequence is then precipitated. Then add adapter and do paired-end sequencing.     
![[../../assets/Mate-pair.png|300]]


#### Phasing error
Phasing means that the blocker of a nucleotide is not correctly removed after signal detection. In the next cycle no new nucleotide can bind on this DNA fragment and the old nucleotide is detected one more time whereby the fluorescence signal of this old nucleotide (probably) differs from the synchronous signal of the other nucleotides.

![[../../assets/phasing.jpg]]

Five strands are on a C-base, whilst 1 is lagging behind (phasing) on a G base and the remaining strand is running ahead of the pack (confusingly called pre-phasing) on an A base. As such the C signal will be reduced and A and G boosted for the rest of the sequencing run.

As a result, this will cause a quality drop over read cycle like: 

![[../../assets/per_base_sequence_quality.png|400]]

#### Barcode
When the library is built, each sample joint is added with a different tag sequence, and then used for sequencing, in the sequencing results, according to the previous label and sample corresponding relationship, can be obtained corresponding sample data.

Simple divided into two types, as follows
+ Inline Barcode: Appears in a base sequence of read (big sample amount)
+ Index Barcode: Appears in the ID section of a read (small sample amount)

![[../../assets/barcode.png|450]]

#### Coverage or depth
##### Mapping depth
测序深度（sequencing depth）或者覆盖深度（depth of coverage）:

![[../../assets/coverage.gif]]

##### Depth
原始测序数据的深度，Depth = (sequenced bases) / (bases of reference) 或者
比对上的数据的深度，Depth= (bases of all mapped reads) / (bases of reference)

##### Covered length
Coverage = (area covered by mapped reads) / (area of reference).

##### Example 
![[../../assets/types-of-coverage.png]]

For complete DNA sample:
$$
\begin{equation}
\begin{aligned}
Depth = (6 * 28nt) / 112nt = 1.2 fold \\ \\
Coverage = (46nt - 5nt) / 112nt = 36.6%
\end{aligned}
\end{equation}
$$
For target gene:
$$
\begin{equation}
\begin{aligned}
Depth = (6 * 28nt) / 46nt = 3.7 fold \\ \\
Coverage = (46nt - 5nt) / 46nt = 89.1%
\end{aligned}
\end{equation}
$$

### Third Generation
**Oxford Nanopore MinION** from 2012:
+ Read-length various from short reading to Super Long Reading; >2Mb recorded 
+ Error rate (randomly): ~5%-10%; 
	+ Improving, for example - R10 pores with ~95% accuracy
> 1Mb = 1000kb = 1,000,000bp

Sequential detection using continuous current signal changes "Directly"

![MinION|500](https://ntuhmc.ntuh.gov.tw/images/58th/Fig2.gif)

This method can greatly increase sequencing speed by differential resolution of nucleotide species by current changes.

![[../../assets/ont-sequencing.png|200]]

The main applications of long fragment sequencing are as follows:

1. Assist de novo genome assembly - the longer the sequence, the easier it is to assemble the DNA
2. Long fragments can be sequenced with a high degree of spanning and repeatability of nucleic acid sequences

Nanopore is not limited to DNA sequencing; it can be used to detect *RNA* and even *proteins*.



## Check sequencing results
In Ubuntu, using FastQC:
```bash
wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.9.zip
sudo apt install unzip
unzip fastqc_v0.11.9.zip
sudo apt install unzip
wget http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/Trimmomatic-0.39.zip 
unzip Trimmomatic-0.39.zip 
rm fastqc_v0.11.9.zip
rm Trimmomatic-0.39.zip

./FastQC/fastqc sequence*.fq
```

The effect of quality trimming - Trimmomatic is a fast multithreaded command line tool for trimming and trimming Illumina (FASTQ) data and removing adapters:
```bash
cp Trimmomatic-0.39/adapters/TruSeq3-SE.fa .

java -jar ./Trimmomatic-0.39/trimmomatic-0.39.jar SE -phred33 sequence2.fq trimmed_sequence2.fq ILLUMINACLIP:TruSeq3-SE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36

./FastQC/fastqc trimmed_sequence2.fq
```