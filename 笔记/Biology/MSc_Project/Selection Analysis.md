# Selection Pressure Analysis

> Selection pressures are external agents which affect an organism’s ability to survive in a given environment

## Intro

个体的表型由其 DNA 序列控制。表型之间的差异是由：
+ 影响基因表达的某些方面的 DNA 序列差异（等位基因）引起的；
+ DNA 序列及其相关调控蛋白的化学修饰引起的，即所谓的 “表观遗传” 变化。

**中性演化理论**全称为**分子演化的中性理论**（Neutral theory of molecular evolution），简称为**中性理论**。

该理论认为，在分子遗传学的层次上，基因的变化大多数是中性突变，也就是对生物个体的生殖与生存既没有好处也没有坏处的突变。由于中性突变并不受自然选择影响，而是由中性的突变基因的*遗传漂变*（genetic drift）产生的
## Codon-substitution models:

过去20年中的一种强大而流行的方法。

密码子则是三个碱基组成的编码单位。在分子进化中，基因序列会随时间不断发生变化，即发生突变，包括非同义替代（导致氨基酸改变的替代）和同义替代（不导致氨基酸改变的替代）。密码子替代模型允许我们通过考虑密码子级别的替代来推断在进化过程中基因发生的变化。

在**一个基因的所有位点和系统发育上的所有谱系** (分支) 上的平均 ω 比率很少大于 1，因此，它用于检测正选择实际上是没有力量的。 **相反**，检测影响**特定分支或单个位点**的正选择被证明更有用。

## 常见的方法

+ 比值分析：计算 $\omega = \frac{dN}{dS}$ 得到 = 1即 (接近) 中性演化，< 1 而表示纯化或负选择，> 1 而则对应于适应性演化或正选择。
+ Codon models：这些模型考虑密码子水平的替代，在研究选择性压力时尤其有用。例子包括 MG94 模型和 GY94 模型等。它比简单的 $\frac{dN}{dS}$ 比值分析更具统计复杂性，因为它们考虑了更多的信息，如密码子使用模式、密码子偏好以及碱基频率等。
+ 位点模型 (Site model)
+ 分支模型 (Branch model):
+ Branch-site模型

### Ratio method 

用于评估编码 DNA 序列中非同义核酸变化速率与同义核酸变化速率的度量。这个度量来自 Kimura描述的序列进化的中性理论。等位基因的随机固定导致了大多数观察到的种间遗传多样性。

估计群体中等位基因固定的突变率 K，有以下公式：
$$K=V_Tf_0$$
$V_T$ → 总突变率，其包括中性、近中性和有害突变。
$f_0$ → 选择性中性或接近中性的突变的分数。

引起密码子中第 $i^{th}$ 位同义变化的可能取代的部分被定义$f_{\alpha i}$, 则非同义取代为$1-f_{\alpha i}$

然后将这些分数应用于每个密码子，估计同义位点$V_s$与非同义$V_A$：
$$v_S=\sum_{i=1}^3f_{\alpha i},v_A=3-v_S$$

考虑从一个密码子（α）到另一个为 β 的密码子的变化。举例来说，从编码苯丙氨酸的密码子 UUU（α）到编码缬氨酸的同源密码子 GUA（β），存在两种可能的路径，被表示为 $p(α,β)$：
$$\begin{aligned}path&1:UUU\left(phe\right)\leftrightarrow GUU\left(val\right)\leftrightarrow GUA\left(val\right)\\\\path&2:UUU\left(phe\right)\leftrightarrow UUA\left(leu\right)\leftrightarrow GUA\left(val\right)\end{aligned}$$
+  两个同源密码子之间的差异是由等于最小替代数(MSN)的突变总量引起的，即同一位点没有反向突变或双重突变。
+ 这两种可能的路径不太可能具有相同的概率。
$$\small\bar{v}_{S},p\left(\alpha,\beta\right)=\frac{(vS,\alpha+vS,\gamma(p)+vS,\delta(p)+vS\beta)}{(MSN+1)}$$
$$\small\bar{v}_{A},p\left(\alpha,\beta\right)=\frac{(v_{A}\alpha+v_{A},\gamma(p)+v_{A}\delta(p)+v_{A}\beta)}{(MSN+1)}$$
其中路径中的中间密码子$γ(p)$ 和 $δ(p)$。对于 α 和 β 之间的任何给定路径，在包括 α 和 β 的路径中存在总共 $MSN + 1$ 个不同的密码子。因此，为了找到平均值，将通过路径中密码子的每种类型的取代的总可能数目除以该数目。

Then, 为了找到从密码子 α 到密码子 β 的同义变化的预期数目$n_S(α,β)$, 将每条路径的权重$\omega_P(α,β)$应用于路径的平均数目$\bar v_S$, $p(α,β)$，并且将所有路径求和；

$$\small n_{S}\left(\alpha,\beta\right)=\sum\limits_{p}^{L}\omega_{p}\left(\alpha,\beta\right)\bar{v}_{S},p\left(\alpha,\beta\right)$$
$$\small n_{A}\left(\alpha,\beta\right)=\sum\limits_{p}^{L}\omega_{p}\left(\alpha,\beta\right)\bar{v}_{A},p\left(\alpha,\beta\right)$$
通过对所有 $n_S$ 和 $n_A$  求和来计算**总**的可能同义 $N_S$ 和总的可能非同义 $N_A$ 取代。然后，对每对同源密码子的实际同义 $M_S$ 和非同义 $M_A$ 取代的数目进行计数。

#### 实际计算
以 CCG 密码子为例：

![[../../assets/CCG.png|500]]

非同义位点的数量 = $0+1/3+1/3+1/3+1/3+0+0+0$ = $1.6666$
同义位点的数量 = $3− 1.666$ = $1.333$

对于一个密码子，其非同义位点和同义位点数量是固定的.

一个5个氨基酸残基的 DNA 序列：ATG – AAA – CCC – GGG – TTT – TAA，其各个密码子的同义和非同义位点数量如下:

![[../../assets/n_s_number.png|500]]

又有一个样本序列：ATG - AAA - CGC - GGC - TAC - TAA

![[../../assets/sample.png|500]]

判断上述序列中的同义突变和非同义突变的数量
第五个密码子比较特别，TTT -> TAC，有两个位点突变。完成该突变的路径有两个：
+ TTT (phe) -> TAT (tyr) -> TAC (tyr)：一个非同义突变 + 一个同义突变
+ TTT (phe) -> TTC (phe) -> TAC (tyr)：一个同义突变 + 一个非同义突变
假设上述两种路径等概率发生，则该观测变异为 1 次同义突变和 1 次非同义突变。

$$\small \begin{aligned} pN &= \frac{2}{14.666} \\\\
pS &= \frac{2}{3.333} \\\\
 dN/dS &= \frac{pN}{pS}=0.2272\end{aligned}$$
常常需要对上述值进行校正，比如使用 JC69 模型进行校正。
$$d_{N/S}=-\frac{3}{4}ln(1-\frac{4p_{N/S}}{3})$$

### Adaptive Branch-Site Random Effects Likelihood (aBSREL)

1. 分支模型 (Branch Model)：
    - 分支模型用于描述不同分支上的进化速率异质性。在进化树上，不同的分支代表了不同的物种或群体，每个分支的进化速率可能不同。
    - 在分支模型中，通常使用不同的进化速率参数来描述不同分支上的替代率差异。这些速率参数可以是固定的，也可以根据数据进行估计。
2. 位点模型 (Site Model)：
    - 位点模型用于描述在蛋白质或基因序列的不同位点上的进化速率异质性。不同位点可能在进化过程中受到不同程度的自然选择作用。
    - 在位点模型中，通常使用不同的参数来表示不同位点的进化速率，如非同义替代率 (dN)和同义替代率 (dS)等。
3. Branch-site 模型 (Branch-site Model)：
    - Branch-site 模型是将分支模型和位点模型结合在一起的模型，用于检测在特定分支上的特定位点是否经历了自适应进化。
    - 这种模型允许在某个特定分支上检测特定位点的非同义替代率 (dN)是否显著大于同义替代率 (dS)。这种情况下，可以认为这些位点可能受到了自然选择的影响。
    - 在 Branch-site 模型中，除了分支和位点的进化速率参数外，还引入了一个 ω 值，表示在特定分支上非同义替代率 (dN)与同义替代率 (dS)的比值。当 ω 值显著大于 1 时，表明特定分支上的特定位点可能经历了自适应进化。

[作者](https://academic.oup.com/mbe/article/32/5/1342/1130440)认为，现有的检测正选择的方法要么过于简单，要么过于复杂，需要一种更加灵活和有效的方法。他们引入了一种自适应分支-位点随机效应似然(aBSREL)方法，该方法允许 ω 在系统发育的两个位点和分支之间变化，但也使用信息标准将模型的复杂性适应于数据。这意味着不同的分支可以有不同数量的 ω 参数，这取决于它们与数据的匹配程度。与其他方法(如分支点 REL (BSREL)和混合效应进化模型(MEME))相比，该方法提高了统计性能，减少了计算时间。

