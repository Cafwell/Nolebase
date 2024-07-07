# Tasks 

You have been provided with a dataset of 51 arthropod Opsin sequences and 4 molluscan outgroups in Fasta format. Your task is to:

(A) Annotate the proteins in the provided data set using online similarity search tools (e.g., NCBI BLAST)    
(B) Expand the dataset to include no more than seven more sequences from two more species: _Drosophila pseudoobscura_ and _Armadillidium vulgare_. Generate a multiple sequence alignment    
(C) a phylogenetic tree for the dataset including the new sequences you added    
(D) Use online tools to identify functional sites and conserved structural domains       
(E) The main arthropod opsin paralogs have been characterised in Drosophila melanogaster and named Rh1, Rh2, Rh3, Rh4, Rh5, Rh6, and Rh7. Identify these seven paralogs in your phylogenetic tree     
(F) Illustrate your findings in the form of a short research paper (including, as appropriate, figures, diagrams and tables), discussing how the ability to detect light of different wavelength evolved in Arthropoda.     

The report should not be longer than 1500 words and should include four sections: (1) Introduction, (2) Methods, (3) Results & Discussion, and (4) References. The report should be fully referenced, but section 4 (References) is not included in the word count. You can include as many figures, diagrams and tables as appropriate. Note however, that the text in figure captions and in tables (including table legends) counts towards the 1500 words limit.

Once completed, your assignment should be submitted to the submission point entitled "Building and Understanding Gene Families".

---

已为您提供了51个Arthropod Opsin序列和4个Molluscan外群的数据集。任务：
1. 使用在线相似性搜索工具（例如NCBI BLAST）在提供的数据集中注释蛋白质
2. 将数据集扩展到不超过两个物种的七个序列：_Drosophila pseudoobscura_ 和 _Armadillidium vulgare_。生成多个序列比对
3. 建立数据集的系统发育树，包括您添加的新序列。
4. 使用在线工具识别功能站点和保守的结构域。
5. 主要节肢动物Opsin旁系同源物在果蝇中的特征是RH1，RH2，RH3，RH4，RH5，RH6和RH7。在系统发育树中识别这七个旁系同源物。
6. 以简短的研究论文的形式说明您的发现（包括适当的图形，图表和表格），讨论了如何检测Arthropoda中不同波长的光的能力
报告不应超过 1500 字，应包括四个部分。报告应有完整的参考文献，但它不计入字数。 您可以根据需要包括尽可能多的图形、图表和表格。 但请注意，图形标题和表格（包括表格图例）中的文本计入 1500 字的限制。

---

## Steps 

1. BLAST for annotation and add some species for extension by using PSI-BLAST (Position-Specific Iterated BLAST)

2. Tree
Use BP system:
```bash
cd /user/work/ms22568/evo/Assessment/
```

```bash
tr " " "_" <Extended.fas> Final.fas  
```

Tree :
```bash
module load apps/muscle/3.8.31
module load apps/trimAI/1.2
module load apps/iqtree/2.1.3

muscle -in final.fas -out final_aln.fas
trimal -in final_aln.fas -out final_aln_trim.phy -automated1
iqtree2 -s final_aln_trim.phys -m MFP -B 1000 -wbt
```

<details>
(KYB28034.1_Opsin_Rh3-like_TC:1.4243409024,XP_015907951.2_rhodopsin_GQ-coupled_PT:0.6297516410,(((((NP_524035.2_rhodopsin_7_DM:0.8069495600,(XP_042908204.1_opsin_Rh4-like_PT:0.6437544843,(XP_013776397.1_LOW-QUALITY-PROTEIN:0.4436254319,(XP_022240090.1_opsin_ultraviolet-sensitive-like_LP:0.1805667077,XP_022241099.1_opsin_9_LP:0.1812054540)100:0.2122298167)83:0.0405405112)99:0.2764124952)100:0.2404418573,((((NP_001285859.1_rhodopsin_5_DM:0.0000021584,XP_001357162.1_opsin_Rh5_DP:0.0675732336)100:0.7106055527,(XP_037790767.1_opsin_ultraviolet-sensitive-like_isoform_X2_PM:0.0000010063,XP_037790766.1_opsin_ultraviolet-sensitive-like_isoform_X1_PM:0.0000010063)100:0.2912063349)43:0.0822377436,(XP_015833491.1_opsin_ultraviolet-sensitive_isoform_X2_TC:0.2277204130,((NP_476701.1_rhodopsin_4_DM:0.0000027443,CAA46711.1_opsin_Rh4_DP:0.0264041770)100:0.1375595505,(NP_524411.1_rhodopsin_3_DM:0.0250806541,XP_001359863.3_opsin_Rh3_DP:0.0290928994)100:0.1398864735)100:0.1140916136)100:0.1901190157)96:0.1143861548,((NP_001301067.1_Opsin_Rh3-like_LP:0.1905011196,XP_042899531.1_opsin_ultraviolet-sensitive_isoform_X3_PT:0.3153247505)100:0.3140092821,(XP_037803464.1_opsin_ultraviolet-sensitive-like_PM:0.1601999515,AWX63624.1_putative_SWS_opsin_AA:0.3115633076)100:0.3291441778)90:0.0901797684)100:0.2142539238)99:0.1541721148,(XP_037792382.1_rhodopsin-like_PM:0.3530720127,XP_037792552.1_lateral_eye_opsin-like_PM:0.2161900268)100:0.6717569295)93:0.0492810764,(((NP_001301099.1_ocellar_opsin-like_LP:0.1363151190,NP_001301068.1_ocellar_opsin-like_LP:0.3736287363)100:0.4505903671,(((NP_524407.1_rhodopsin_1_DM:0.2022785502,(NP_524398.1_rhodopsin_2_DM:0.0139217515,XP_001358861.1_opsin_Rh2_DP:0.0419971870)100:0.2159117047)100:0.3062157363,(((NP_524368.5_rhodopsin_6_DM:0.0000022918,XP_002137154.1_opsin_Rh6_DP:0.0258430641)100:0.4365567101,NP_001155991.1_rhodopsin_1/6-like_TC:0.1383070167)65:0.0606466369,(((XP_037784807.1_rhodopsin-like_PM:0.0597173901,XP_037784803.1_rhodopsin-like_PM:0.1212255894)100:0.2422043037,AWX63623.1_putative_LWS_opsin_AA:0.2514755047)100:0.1403625788,(RXG72077.1_Opsin_1_AV:0.2525187317,((XP_037785742.1_rhodopsin-like_PM:0.0736592641,(((XP_037786268.1_rhodopsin-like_PM:0.2052271641,XP_037785722.1_rhodopsin-like_PM:0.0000010063)99:0.0056605689,XP_037785721.1_rhodopsin-like_PM:0.0107929754)92:0.0180283724,XP_037785719.1_rhodopsin-like_PM:0.0216663661)92:0.0229868416)78:0.0200683332,(((((XP_037786269.1_rhodopsin-like_PM:0.0908211630,(XP_037785718.1_rhodopsin-like_PM:0.0327302047,XP_037785711.1_rhodopsin-like_PM:0.0127337666)99:0.0418366502)23:0.0000028167,XP_037785716.1_rhodopsin-like_PM:0.0000010063)43:0.0150782261,XP_037785696.1_rhodopsin-like_PM:0.0072474878)46:0.0150670935,XP_037785684.1_rhodopsin-like_PM:0.0000024787)43:0.0034125877,XP_037785685.1_rhodopsin-like_PM:0.0076316764)68:0.0490104987)100:0.1348566203)99:0.0880231383)97:0.0985826674)84:0.0465962940)98:0.1108931278,(((XP_015911002.1_ocellar_opsin_PT:0.4133643085,(XP_042913270.1_ocellar_opsin-like_PT:0.0091501283,XP_042913271.1_lateral_eye_opsin-like_PT:0.0717365372)100:0.1405016496)54:0.0490465913,(P35360.1_Lateral_eye_opsin_LP:0.0096733678,NP_001301089.1_ocellar_opsin_LP:0.0048101316)100:0.0895432198)94:0.0991080627,NP_001301103.1_ocellar_opsin-like_LP:0.2671626593)86:0.0691869754)78:0.0677494090)100:0.2704244733,(NP_001301044.1_ocellar_opsin-like_LP:0.3788704353,((XP_037780172.1_compound_eye_opsin_BCRH2-like_PM:0.0664969210,XP_037780897.1_compound_eye_opsin_BCRH2-like_PM:0.0818521057)100:0.3648369757,(XP_037783167.1_compound_eye_opsin_BCRH1-like_PM:0.2095881942,XP_037785902.1_compound_eye_opsin_BCRH1-like_PM:0.2832816205)100:0.2137686004)89:0.0962865873)95:0.0686155379)89:0.0663845268)100:0.2855466692,(CAG2217447.1_OPN4_ME:0.3626802090,(KAI8794215.1_rhodopsin-like_BG:0.3647628255,(QKY89065.1_Rhabdomeric_opsin_CM:0.0358479714,APF30601.1_opsin_AG:0.0441458794)100:0.1825637798)97:0.1047726832)100:0.1830671574)99:0.3423151578);
</details>

3. Running [Web CD-search](https://www.ncbi.nlm.nih.gov/Structure/bwrpsb/bwrpsb.cgi?):
Find Conserved Domain families: Seven-transmembrane G protein-coupled receptor superfamily or in short [**7tm_GPCRs**](https://www.ncbi.nlm.nih.gov/Structure/cdd/cddsrv.cgi).

4. Tree stuff:
Fine outgroups (AG) and reroot. 
**4 outgroups are:**    
Chiton marmoratus - 花斑石鳖 - CM     
Acanthopleura granulata - 西印度石鳖 - AG     
Biomphalaria glabrata - 紅扁蜷 - BG    
Mytilus edulis - 紫贻贝 - ME    

5. Visualise 
Use Figtree or [iTOL](https://itol.embl.de/), annotate Rh 1-7

Next:
Now that you have your annotated phylogenetic tree, you can start by examining the patterns of opsins gene <u>duplication and loss</u> in different arthropod groups. Look for branches that show evidence of gene duplication or loss events, such as nodes with multiple branches leading to different groups of arthropods, or branches with long or short lengths that indicate changes in gene number or sequence.

Once you have identified the main patterns of gene duplication and loss, you can discuss the functional implications of these changes in gene number and sequence on the spectral sensitivity of different arthropod groups. For example, you may compare the number and type of opsins genes in different arthropod groups and discuss how these changes may reflect the ecological niche and visual needs of different species.

You can also look at the expression patterns of opsins genes in different arthropod groups and discuss how these patterns may differ between groups, such as in the distribution and abundance of photoreceptor cells in the eyes or other light-sensitive organs.

Next, you can consider the role of natural selection and environmental factors in shaping the evolution of opsins proteins and the visual systems of different arthropod groups. For example, you may discuss how the spectral sensitivity of different arthropod groups may be linked to their ecology and behaviour, such as the role of UV detection in pollination or mate choice, or the importance of green or red light detection in predator avoidance.

You can also discuss how environmental factors, such as changes in light intensity or quality, may have influenced the evolution of opsins proteins and the visual systems of different arthropod groups, such as through shifts in selective pressures or changes in gene expression. Consider how the evolutionary history of arthropod groups may have affected their visual systems, and how these changes may have contributed to their ecological success and adaptation to different environments.
