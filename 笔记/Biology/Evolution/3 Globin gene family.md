# Pre-steps 

Login 
```bash
ssh ms22568@bp1-login.acrc.bris.ac.uk
cd /user/work/ms22568/evo/bioinf_practicals/globins/
```

# Task1

![[../../assets/Phylogeny_of_species_globins.png]]

The task is to generate a Globin gene tree, and functionally classify the paralogs as best as possible.

Formatting 

```bash
sed "s/^>\(.*\)\[\([[:alpha:]]* [[:alpha:]]*\)\]/>\2_\1/g" <globin.fas> globin_s.fas  

tr " " "_" <globin_s.fas> globin_st.fas  

tr ":" "_" <globin_st.fas> globin_st2.fas 

awk '(/^>/ && s[$0]++){$0=$0"_"s[$0]}1;' globin_st2.fas > globin_done.fas
```


```bash
module load apps/muscle/3.8.31
module load apps/trimAI/1.2
module load apps/iqtree/2.1.3

muscle -in globin_done.fas -out globin_done_aln.fas
trimal -in globin_done_aln.fas -out globin_done_aln_trim.phy
iqtree2 -s globin_done_aln_trim.phy -m MFP -B 1000
```


