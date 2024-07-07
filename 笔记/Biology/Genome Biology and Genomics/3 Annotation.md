# Overview

Genome annotation is the process of finding *genes* and other *elements* on a  
genome sequence, and working out what they do.  

Two steps:  
+ **Structural** annotation (where are the genes, or other elements?)  
+ **Functional** annotation (what do these genes do?)

## Structural annotation

### Viruses and Prokaryote 
Viral and prokaryotic genomes are small and well-organised ([[2 Genome Assembly#Viruses|Virus]] in last course)

Prokaryotic gene structure is fairly straightforward ([[2 Genome Assembly#Prokaryotic|Prokaryotic]]), gene structure is conserved at the sequence level;

Can Inferring gene structures directly from the sequence:

![[../../assets/Direct_gene_structures.png]]

For prokaryotic structural annotation, there are some leading gene finding tools such as **Prodigal**, using other sources of information from codon usage bias, nucleotide usage patterns for each codon position, to identify likely genes.

### Eukaryotes 
Not feasible (可行的) from the genome sequence alone, need the help to inform gene predictions:
+ Transcriptome data  
+ Protein sets from related species  

## Functional annotation
> What are the functions of the genes on a genome, what are the  functional/metabolic capabilities of the organism?

### Prokaryotic 
Given a set of ORFs, use **evolution** can predict their functions.


# Practice 

Structural and functional annotation of a small prokaryotic genome (*Nasuia deltacephalinicola*)

BUSCO: Loading BUSCO requires loading three modules in the correct order, and then activating a conda environment (that’s bioinformatics for you…).
```bash
module add lang/python/anaconda/3.8-2021-TW
module add lang/perl/5.30.0-bioperl-TW
module add apps/blast/2.2.31+
source activate busco-5.0.0
```

Once you have *finished* using BUSCO, you should deactivate the environment:
```bash
conda deactivate
```

And then load prokka: 
```bash
module add lang/python/anaconda/3.8-2021-TW
module add lang/perl/5.30.0-bioperl-TW
module add apps/blast/2.2.31+
```


```
busco --in nasuia.fasta --mode genome --lineage bacteria_odb10 --out output_dir
```

Then perform structural annotation of the Nasuia genome using Prokka. Both programs take the genome assembly (`nasuia.fasta`) 

```bash
prokka --kingdom Bacteria nasuia.fasta
```





