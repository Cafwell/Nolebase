Two questions before course:

1. How are differences in the biology of organisms (their features, traits, capabilities, ways of life) reflected in their genomes? (Why do genomes vary?)
2. What factors would affect how we should approach sequencing and assembling different kinds of life forms?  


# Diversity of genome structures 

## Viruses 
Genomes size: ~1.7Kb - 2Mb
Characters: compact, may have overlapping genes and *polyproteins*.

> polyproteins, a replication strategy for viruses, which can be cleaved by viral and/or cellular proteases.

![[../../assets/polyprotein.png|400]]


## Prokaryotic
Genomes size: ~200Kb - 16Mb
Characters: 
+ Often single, circular molecule
+ gene dense 
+ often continue (no introns)

![[../../assets/Prokaryotic_Genomes.png|400]]

Gene often organized into operons (操纵子), one example is lactose operon of E.coli
![[../../assets/operon.png|400]]

Promoter is **highly conserved** with two motifs.
![[../../assets/Promoter.png|400]]

Shine-Dalgarno sequence (ribosome binding site) also highly conserved.
![[../../assets/Shine-Dalgarno.jpg|400]]

## Eukaryotes
Genomes size: ~2.3Mb - ??Mb; (human-~3100Mb; marbled lungfish-~132800Mb)

Where is the difference? - non-coding DNA!
![[../../assets/non-coding.jpg|400]]

And eukaryotes genomes contain a body of junk DNA.

# Basic pipeline for genome assembly
![[../../assets/Assembly_steps.jpg|400]]

Workflow:
1. Sequence (Short or Long reads)
2. Check quality, trim low-quality bases and adapters (using **FastQC**; **trimmomatic**)
3. Assemble reads 
	1. Short: **MEGAHIT**, **SPAdes**
	2. Long: **Canu**, **Flye**, **Redbean**, **Shasta**
3. Check assembly quality {**Kraken** (taxonomy), **QUAST** (assembly statistics)}
4. Annotate and analyze 

## Two reads types 
Long and Short:


# Viral genome assembly from Illumina MiSeq short read


```bash
module load tools/git
cd /user/work/ms22568/
git clone https://github.com/Tancata/GenomeBiologyandGenomics.git
```

The modules needed for this are:
**FastQC**: `module add apps/fastqc/0.11.9`
**Trimmomatic**: `module add apps/trimmomatic/0.39`
`java -jar /sw/apps/Trimmomatic-0.39/trimmomatic-0.39.jar`
**MEGAHIT**: `module add lang/python/anaconda/3.8-2021-TW`
**QUAST**: `module add lang/python/anaconda/3.8-2021-TW`
**Prokka**: Loading prokka requires loading three modules in the correct order. (If you get things wrong, use module unload moduleName to remove the offending module and start again):
`module add lang/python/anaconda/3.8-2021-TW
`module add lang/perl/5.30.0-bioperl-TW`
`module add apps/blast/2.2.31+`

## Use FASTQC 
To evaluate the quality of the sequence data
```bash
cd /user/work/ms22568/GenomeBiologyandGenomics/session2/
fastqc virus_all_R1.fastq virus_all_R2.fastq
```

The FastQC output files are written to a zip archive, which can be unzipped using the unzip
command.
(a) Do the data contain adaptors, or other over-represented k-mers?
No
(b) Do the data contain low-quality bases? Errors in base calls interfere with de Bruijn-graph based assembly.
No

## Use Trimmomatic 
To delete any low-quality (score < 3) bases from the read data

```bash
java -jar /sw/apps/Trimmomatic-0.39/trimmomatic-0.39.jar PE virus_all_R1.fastq virus_all_R2.fastq output_1_paired.fastq output_1_unpaired.fastq output_2_paired.fastq output_2_unpaired.fastq LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15
```
Then Use FastQC again.

## Assemble the reads (MEGAHIT)
The input (forward and reverse) files for this step will be the `output_1_paired` and `output_2_paired` files from the trimming step.

```bash
/sw/lang/anaconda.3.8-2021-TW/bin/megahit -1 output_1_paired.fastq -2 output_2_paired.fastq -o assembly_all_contigs
```

## Use QUAST
Compute basic assembly statistics (N50 and L50) from the assembly 

```bash
quast.py assembly_all_contigs/final.contigs.fa
```

N50  29901   
N90  659   
L50  1   

> N50 和 L50 是 Contigs (重叠群) 或脚手架长度的统计值。
> N50：可以近似的看作分布质量一半的点，对长 Contigs 的权重更大
> L50：长度总和占基因组大小一半的最小 Contigs 数

![[../../assets/N50_and_L50.png|400]]

## Prokka
Do basic annotation on the viral contigs.
```bash
prokka --kingdom Viral final.contigs.fa
```

Confirm your taxonomic classification by inspecting the gene annotations produced by prokka, and using [NCBI protein BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi?LINK_LOC=blasthome&PAGE_TYPE=BlastSearch&PROGRAM=blastn).

## (Optional) visualise the gene models 

Use a genome viewer software such as [Artemis](http://sanger-pathogens.github.io/Artemis/Artemis)) (Sanger) or [Integrative Genomics Viewer](http://software.broadinstitute.org/software/igv/) (IGV; Broad) to do this.

[ASCIIGenome](https://asciigenome.readthedocs.io/en/latest/) is a command line alternative, if you prefer to work in the terminal.

