
# Pre-steps

Log in

```bash
ssh ms22568@bp1-login.acrc.bris.ac.uk
```

Use `ald.fas` file in:

```bash
cd /user/work/ms22568/evo/bioinf_practicals/7_genes/Aldolase
```

# Multiple sequence alignment

Muscle, MAFFT, Clustal and other programs can do that. Here use Muscle

```bash
module load apps/muscle/3.8.31
muscle -h 
```


```bash
muscle -in ald.fas -out ald_aln.fas
```

# Remove poorly aligned sites

Use TrimAl with an output file to be in Phylip4 format

```bash
module load apps/trimAI/1.2
trimal -in ald_aln.afa -out ald_aln_trim.phys
ls
```

# Build a phylogenetic tree

The two most popular software for Maximum Likelihood reconstruction are RAxML and IQtree. For this practical will be using IQtree.

+ IQtree allows choosing among a larger number of models (including complex heterogeneous models). 
+ However, RAxML is a better software from an engineering point of view, which means that it run faster and can handle larger datasets.


```bash
module load apps/iqtree/2.1.3
iqtree2 -s ald_aln_trim.phy -m MFP -B 1000
```

> -m：模型选择，设置 MF 自动选择最佳模型但不建树；设置 MFP 自动检测最佳模型并建树。此外还可以设置具体的模型，或者多个可选模型，例如 - m LG,WAG    
> -B：超快速 bootstrap 次数，大于等于 1000

Result:

```output
Performs final model parameters optimization
Estimate model parameters (epsilon = 0.010)
1. Initial log-likelihood: -7705.951
Optimal log-likelihood: -7705.951
Proportion of invariable sites: 0.196
Gamma shape alpha: 0.950
Parameters optimization took 1 rounds (0.052 sec)
BEST SCORE FOUND : -7705.951
Creating bootstrap support values...
Split supports printed to NEXUS file ald_aln_trim.phy.splits.nex
Total tree length: 10.779

Total number of iterations: 154
CPU time used for tree search: 54.114 sec (0h:0m:54s)
Wall-clock time used for tree search: 54.076 sec (0h:0m:54s)
Total CPU time used: 57.654 sec (0h:0m:57s)
Total wall-clock time used: 57.524 sec (0h:0m:57s)

Computing bootstrap consensus tree...
Reading input file ald_aln_trim.phy.splits.nex...
58 taxa and 556 splits.
Consensus tree written to ald_aln_trim.phy.contree
Reading input trees file ald_aln_trim.phy.contree
Log-likelihood of consensus tree: -7705.951

Analysis results written to:
  IQ-TREE report:                ald_aln_trim.phy.iqtree
  Maximum-likelihood tree:       ald_aln_trim.phy.treefile
  Likelihood distances:          ald_aln_trim.phy.mldist

Ultrafast bootstrap approximation results written to:
  Split support values:          ald_aln_trim.phy.splits.nex
  Consensus tree:                ald_aln_trim.phy.contree
  Screen log file:               ald_aln_trim.phy.log
```

The file ending in **`.iqtree`** is the most important summary output file, including: 
+ Details of the substitution model used;
+ The maximum likelihood tree (i.e. the optimal tree for this data set and the consensus tree obtained summarising the trees obtained in 1000 fast bootstrap replicates).  

To get files from ssh, we can use `WinSCP` in windows 

For Linux/Mac:

```bash
scp ms22568@bp1-login.acrc.bris.ac.uk:/user/work/ms22568/bioinf_practicals/7_genes/Aldolase/ald_aln_trim.phy.treefile
```


## Visualization
Download  [Figtree](https://github.com/rambaut/figtree/releases) in local computer (not in BP), copy the maximum likelihood tree and paste it in the Figtree window.

Or use ggtree package in R

Install (Biocmanager installation recommended ):
```R
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install("ggtree", version = "3.8")
```

Drawing plot:
```R
library(treeio)
library(ggplot2)
library(ggtree)
tree <-read.newick(".../ald_aln_trim.phy.treefile", # file loca
                  node.label = "support") # node-label 解析为 support value
                  
ggtree(tree) +
geom_treescale(x=0.9,y=0,color="red")+geom_tiplab() + 
theme_tree2() +  #显示坐标轴范围  
geom_text(aes(subset=!isTip, label= support, color=support),hjust=-.2,vjust=0.5) +
theme(legend.position = "none")
```
![[../../assets/Aldolase.png]]