# Pre-steps

Log in
```bash
ssh ms22568@bp1-login.acrc.bris.ac.uk
```

Use `ald.fas` file in:
```bash
cd /user/work/ms22568/evo/bioinf_practicals/7_genes/
```

# Repeat 

For each Fasta file in the genes directory, you will need to:
1. Build a multiple sequence alignment.
2. Trim the multiple sequence alignment
3. Build a phylogenetic tree (using IQtree under the best fitting model and estimating support using ultrafast bootstrap : 1000 replicates).
4. Inspect the content of each of the `.iqtree` files to find the optimal tree and save it into a single summary text file
5. Copy and paste the content of the summary file in `FigTree` to visualise and compare the 7 trees.

For doing steps automatically, we use bash script named Analysis_automator.sh:

```bash
#!/bin/bash

#SBATCH --job-name=Automator
#SBATCH --nodes=1
#SBATCH --account=gely028645
#SBATCH --cpus-per-task=1
#SBATCH --time=2:00:00
#SBATCH --mem=10G
#SBATCH --partition=tech_cpu
#directory
job_dir=$( pwd )
cd $job_dir


#Script to analyse and concatenate the seven gene data set
#Script will build individual gene trees

 
module load apps/iqtree/2.1.3
module load apps/muscle/3.8.31
module load apps/trimAI/1.2

echo "running script"
for i in *fas
do
    echo "runnning MATTF on file $i"
    muscle -in $i -out  $i.aln.fas
    echo "done"
done

for m in *aln.fas
do
trimal -in $m -out $m.trim.phy -phylip
done


for n in *phy
do 
    echo "running model selection and phylogenetic analyses with ultrafast bootstrap 1000 replicates in IQtree2 for file $m"
    iqtree2 -s $n  -m MFP -B 1000
    echo "analysis of file $m completed"
done
echo "All trees built!"

for n in *iqtree
do
    grep -A2 "Consensus tree in newick format:" < $n | grep "(" >> 7_ConsensusTrees.trees
done

echo "All done!"

```

To run it, use:
```shell
sh Analysis_automator.sh
```


# Clear numbers 

```
sed "s/ [[:digit:]]* [[:alpha:]]*//g" ald.fas.trim.fas
```

# Super-alignment

Use [FASconCAT-G](<(https://github.com/PatrickKueck/FASconCAT-G)>)

Folder `/concatenated_genes` with `FASconCAT-G`  script, copy `.phy` files into the folder.
```bash
cp *phy concatenated_genes
cd concatenated_genes
```

Then running:
```bash
perl FASconCAT-G_v1.04.pl -p -p -s
```
Will get the file called `FcC_supermatrix.phy`

Next:
```bash
module load apps/iqtree/2.1.3
iqtree2 -s FcC_supermatrix.phy -m MFP -B 1000
```

After a long time, get the result 
```
Analysis results written to:
  IQ-TREE report:                FcC_supermatrix.phy.iqtree
  Maximum-likelihood tree:       FcC_supermatrix.phy.treefile
  Likelihood distances:          FcC_supermatrix.phy.mldist

Ultrafast bootstrap approximation results written to:
  Split support values:          FcC_supermatrix.phy.splits.nex
  Consensus tree:                FcC_supermatrix.phy.contree
  Screen log file:               FcC_supermatrix.phy.log
```

# Plot:
```R
library(treeio)
library(ggplot2)
library(ggtree)
tree_super <- read.newick(".../FcC_supermatrix.phy.treefile", node.label = "support")
ggtree(tree_super, branch.length = "none",layout = "circular") + 
  geom_tiplab(size=3,align = TRUE,offset = 1) + # 添加虚线
  geom_nodepoint(aes(size=support,x=x-0.5),
                 color="#8f8fc3") +
  theme(legend.position = c(0.05,0.15))
```
![[../../assets/super_alignment_circle.png]]

Or use online tool - [Chiplot](https://www.chiplot.online/#Phylogenetic-Tree):
![[../../assets/super_alignment.png]]
