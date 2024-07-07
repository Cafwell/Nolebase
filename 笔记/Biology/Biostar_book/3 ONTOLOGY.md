# Ontology?

An _ontology_ is a controlled vocabulary of _terms_ or _concepts_ and a restricted set of _relationships_ between those terms. The most commonly used ontologies for bioinformatics data are:  
本体是由术语或概念组成的受控词汇，以及这些术语之间的有限关系集。最常用于生物信息学数据的本体是：
-   The Sequence Ontology (`SO`): a vocabulary for information related to sequence features.  
    序列本体（ `SO` ）：与序列特征有关的信息的词汇。
-   The Gene Ontology (`GO`), a vocabulary for information related to gene functions.  
    基因本体论（ `GO` ），是一个与基因功能有关的信息词汇。

本体是指是在一定知识范围内，对所谈论的一组概念，对它们给出一种语义约定。

## Sequence ontology 

The Sequence Ontology (abbreviated as SO) located at [here](<(http://www.sequenceontology.org/)>).

用于定义生物序列注释中使用的序列特征。大多数数据源会用本体论中定义的词来标记信息。
```bash
wget http://data.biostarhandbook.com/data/SGD_features.tab
```

```
$ cat SGD_features.tab | cut  -f 2 | sort-uniq-count-rank | head
7074    CDS
6604    ORF
484     noncoding_exon
383     long_terminal_repeat
377     intron
352     ARS
299     tRNA_gene
196     ARS_consensus_sequence
91      transposable_element_gene
77      snoRNA_gene
```

>Search the [Sequence Ontology Browser](http://www.sequenceontology.org/browser/obob.cgi) for the words that need to be defined.

最常见的CDS，定义可找到于[这儿](http://www.sequenceontology.org/browser/current_release/term/SO:0000316)

| **SO Accession** | SO:0000316 (SOWiki)                                                                                                |
|--------------|---------------|
| **Definition**   | A contiguous sequence which begins with, and includes, a start codon and ends with, and includes, a stop codon.  |
| **Synonyms**     | coding sequence, coding_sequence, INSDC_feature:CDS                                                              |
| **DB Xrefs**     | SO: ma        |

在 [https://github.com/The-Sequence-Ontology/SO-Ontologies](https://github.com/The-Sequence-Ontology/SO-Ontologies) 有一个 GitHub 存储库，有着obo格式的OS条目。

```bash
wget https://raw.githubusercontent.com/The-Sequence-Ontology/SO-Ontologies/master/Ontology_Files/so-simple.obo
```

```bash
cat so-simple.obo | grep 'Term' | wc -l
```

返回结果为 `2359` ，表明序列本体有着 `2359` 术语。

### Search the Sequence Ontology

```bash
cat so-simple.obo | grep 'name: gene'
```
prints:
```bash
name: gene_sensu_your_favorite_organism
name: gene_class
name: gene_part
name: gene_by_transcript_attribute
name: gene_by_polyadenylation_attribute
name: gene_to_gene_feature
name: gene_array_member
```

只匹配 `gene`，并打印打印匹配前后的行：
```bash
cat so-simple.obo | grep 'name: gene$' -B 1 -A 6
```
will print:
```bash
id: SO:0000704
name: gene
def: "A region (or regions) that includes all of the sequence elements necessary to encode a functional transcript. A gene may include regulatory regions, transcribed regions and/or other functional sequence regions." [SO:immuno_workshop]
comment: This term is mapped to MGED. Do not obsolete without consulting MGED ontology. A gene may be considered as a unit of inheritance.
subset: SOFA
xref: http://en.wikipedia.org/wiki/Gene "wiki"
is_a: SO:0001411 ! biological_region
relationship: member_of SO:0005855 ! gene_group
```

## Gene ontology
本体论的目的是对**基因的产物**进行分类，而不是对基因本身。同一基因的不同产物可能发挥非常不同的作用。

Gene Ontology 的结构是一个有向无环图，有点类似于分类树；节点是 GO 项，并且边是这些项之间的关系。每个子节点都比其父节点更 “具体”，并且函数是 “继承” 下来的。

主要的关系有以下几种：

|  | |
| :--------------: | :--------------: |
| is a                 | part of              |
| regulates            | negatively regulates |
| positively regulates | occurs_in            |
| has_part             |                      |

GO 有着三个结构化的独立子本体
-   **Where does the product exhibit its effect?** -> `Cellular Component (CC)` 
-   **How does it work?** -> `Molecular Function (MF)`
-   **What is the purpose of the gene product?** –> `Biological Process (BP)` 

每个子图都是独立的：

![[../../assets/go-trees.png|500]]

各种组织已经承担了可视化 GO 术语的任务，在可用性和信息性方面取得了各种成功：
-   [GeneOntology.org](http://www.geneontology.org/) is the “official” data originator.  
-   [Quick Go](https://www.ebi.ac.uk/QuickGO/) by EMBL
-   [AmiGO](http://amigo.geneontology.org/amigo) Gene Ontology browser  

### Understanding the GO Data
```shell
curl -OL http://purl.obolibrary.org/obo/go.obo
```

类似地，文件的内容是一种面向记录的格式，这意味着每个信息块都分布在多行上，形式如下：
```bash
$ grep --color=AUTO -B 2 -A 3 'name: mitochondrial genome maintenance' go.obo
[Term]
id: GO:0000002
name: mitochondrial genome maintenance
namespace: biological_process
def: "The maintenance of the structure and integrity of
  the mitochondrial genome; includes replication and
  segregation of the mitochondrial chromosome." [GOC:ai, GOC:vw]
is_a: GO:0007005 ! mitochondrion organization
```

### GO association file
关联文件将基因产物连接到 GO 术语：
```bash
IGLV3-9    ...     GO:0005886
IGLV3-9    ...     GO:0003823
IGLV3-9    ...     GO:0002250
...
```

这将意味着基因产物用术语和注释。的定义可以在以下文件中找到：
```bash
cat go.obo | grep "id: GO:0005886"  -A 5
```

```bash
curl -OL http://geneontology.org/gene-associations/goa_human.gaf.gz
```

#### GAF格式
如下：
```bash
UniProtKB   A0A024QZP7   CDC2  GO:0004672  GO_REF:0000002 ...
```

前几列指示基因产物名称，然后列出其对应的 GO 关联。由于 GO 术语具有分层表示，因此基因名称的 GO 术语意味着基因也用所有父节点注释。父节点通常描述类似的特征，但在更广泛的意义上。在基因关联文件中，每个基因产物可以与每个类别中的一个或多个 GO 术语相关联。

### Properties of GO data
```bash
# What are the ten most highly annotated proteins in the GO dataset?
cat goa_human.gaf | cut -f 2 | sort-uniq-count-rank > prot_counts.txt

# What are the ten most highly annotated gene names in the GO dataset?
cat goa_human.gaf | cut -f 3 | sort-uniq-count-rank  > gene_counts.txt
```

检查 `gene_counts.txt` :
```bash
cat gene_counts.txt | head
```

```bash
1102 HTT
930 TP53
858 EGFR
839 GRB2
680 GOLGA2
674 APP
607 CTNNB1
606 UBC
584 TRAF2
548 RPS27A
```

使用`datamash`的小工具可以统计：
```bash
# Compute the mean for column 1.
cat gene_counts.txt | datamash  mean 1
32.255854293148
```

```bash
# Compute the mean, min, max and standard deviations for column 1.
cat gene_counts.txt | datamash min 1 max 1 sstdev 1
1   1098    45.351828648594
```

### Bio explain
```bash
pip install bio --upgrade
bio --download
```

```bash
bio explain exon
```

Will produce:
```bash
## exon (SO:0000147)

A region of the transcript sequence within a gene which is not removed from the
primary RNA transcript by RNA splicing.

Parents:
- transcript_region

Children:
- coding_exon
- noncoding_exon
- interior_exon
- decayed_exon (non_functional_homolog_of)
- pseudogenic_exon (non_functional_homolog_of)
- exon_region (part_of)
- exon_of_single_exon_gene
```

知道标识符时，可以直接使用 `bio explain GO:2000147` 等
### Term lineage
```bash
bio explain exon --lineage
```
prints: 
```bash
SO:0000110  sequence_feature
  SO:0000001  region
    SO:0001411  biological_region
      SO:0000833  transcript_region

        ## exon (SO:0000147)

        A region of the transcript sequence within a gene which is not removed from the
        primary RNA transcript by RNA splicing.


        Children:
        - coding_exon
        - noncoding_exon
        - interior_exon
        - decayed_exon (non_functional_homolog_of)
        - pseudogenic_exon (non_functional_homolog_of)
        - exon_region (part_of)
        - exon_of_single_exon_gene
```

## Functional analysis

功能分析通常意味着在解释观察到的现象，支持或拒绝假设，发现机制的背景下解释数据。

### Over-Representation Analysis (ORA)
过度表达分析（ORA）试图找到一系列基因的代表性功能。它通过将实验中观察到的函数的次数与基线进行比较来做到这一点。它是鉴定一组基因和转录物中的共同功能的最常用的方法之一。