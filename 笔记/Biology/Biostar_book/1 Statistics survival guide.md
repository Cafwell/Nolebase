
# What is statistics ?

统计学类似于盲人摸象。每个人都在使用，而且往往只知道一套特定的统计工具，在传授他们的知识时，并没有传授统计的一般结构。  

然而，这种结构只包括四个部分：计划实验和试验；探索所得数据的模式和结构；从该数据中做出可重复的推论；以及设计与分析结果互动的经验。这些部分在该领域被称为
-   Design of experiments 实验设计
-   Exploratory data analysis 探索性数据分析
-   Inference 推断
-   Visualization 可视化

## Data: 
```tab
Name         C1        C2        C3        P1       P2      P3
TP53     1887.7       8.1    1799.6     800.2    785.3   800.4
SEZ6L      9.9        8.9       7.7     400.1    389.3   403.9
MICALL1   236.4       8.4     220.9     127.8    156.3   140.2
...
```

## Pairwise comparisons 成对比较
生物学中最常用的统计检验是所谓的成对比较, 它不仅比较单个数字，而且比较相同条件下的重复测量（called _replication_）。通常，通过形成它们的差来比较两组数字；然后计算公式来表示观察到的差异是由于随机机会引起的似然性。这种可能性就是臭名昭著的 [p值](https://www.zhihu.com/question/23149768)。

## Normalization 归一化
为了进行任何比较，我们测量的值必须反映相同的量值。将把数据变成 (0, 1) 或者 (1, 1) 之间的小数。

相关文章：
1.  [Normalization(min-max)](https://senitco.github.io/2017/09/12/deep-learning-normalization/#Normalization-min-max)
2.  [Standardization](https://senitco.github.io/2017/09/12/deep-learning-normalization/#Standardization)
3.  [ZCA Whitening](https://senitco.github.io/2017/09/12/deep-learning-normalization/#ZCA-Whitening)
4.  [Global Contrast Normalization(GCN)](https://senitco.github.io/2017/09/12/deep-learning-normalization/#Global-Contrast-Normalization-GCN)
5.  [Local Contrast Normalization(LCN)](https://senitco.github.io/2017/09/12/deep-learning-normalization/#Local-Contrast-Normalization-LCN)
6.  [Local Response Normalization(LRN)](https://senitco.github.io/2017/09/12/deep-learning-normalization/#Local-Response-Normalization-LRN)
7.  [Batch Normalization](https://senitco.github.io/2017/09/12/deep-learning-normalization/#Batch-Normalization)
8.  [Layer Normalizaiton](https://arxiv.org/pdf/1607.06450v1.pdf)
9.  [Instance Normalization](https://arxiv.org/pdf/1607.08022.pdf) ; [https://github.com/DmitryUlyanov/texture_nets](https://github.com/DmitryUlyanov/texture_nets)
10.  [Group Normalization](https://arxiv.org/pdf/1803.08494.pdf)
11.  [Switchable Normalization](https://arxiv.org/pdf/1806.10779.pdf);  [https://github.com/switchablenorms/Switchable-Normalization](https://github.com/switchablenorms/Switchable-Normalization)

## Effect size 效应量
效应量是一种度量效应大小的指标．效应量具有与测量单位无关、单调性、不受样本容量的影响等基本性质。效应量可以解决 P 值无法刻画相关程度大小和差异大小的问题，也可以避免 “P 值操控” 现象。

效应量衡量实验真实效果大小或者变量关联强度的指标，它不受样本容量大小的影响 

依据效应量的大小，能够判断具有显著差异的研究结果是否具有实际意义或重要性。

P 值代表的是统计学上的意义（p 值为 0.0001 表明，如果零假设为真，则数据像被测试数据极端或比被测试数据更极端的概率为万分之一），而效应量是能反映实际上的意义，有时候即使有显著的统计学意义，但是效应量却可以很小。

对应于效应量的值可以用明确定义的含义（例如，相关性、百分比、比值）标准化，或非标准化地计算为组平均值之间的差异。

如果我们比较基因 `TP53`
```R
x = c(1887.7, 8.1, 1799.6)
y = c(800.2,  785.3,  800.4)

print( mean(x) - mean(y))
```

Produces the effect size: 
```R
453
```

See also the blog post: [Effect size is significantly more important than statistical significance.](http://www.argmin.net/2021/09/13/effect-size/)

若使用t-test：
```R
t = t.test(x,y)
print (t$p.value)
```
prints:
```R
0.5499189
```

上述检验告诉我们，使用 t 检验时，偶然观察到效应量 `453` 的可能性为 `0.54` （54%）

## Wilcoxon 检验
也被称为 Mann-Withney-Wilcoxon 检验，是一种*非参数检验*，意味着它不依赖于属于任何特定参数的概率分布家族的数据。非参数检验的目标与参数检验的目标相同。然而它*不需要假设分布的正态性*。

```R
dat <- data.frame(
  Sex = as.factor(c(rep("Girl", 12), rep("Boy", 12))),
  Grade = c(
    19, 18, 9, 17, 8, 7, 16, 19, 20, 9, 11, 18,
    16, 5, 15, 2, 14, 15, 4, 7, 15, 6, 7, 14
  )
)
w = wilcox.test(dat$Grade ~ dat$Sex)
print (w$p.value)
---
0.0205607
```

此分布不符合正态分布，应使用非参数检验。

H0: 两组是相似的     
H1: 两组是不同的     

结果P 值为 0.02，因此，在 5% 显著性水平下，我们拒绝原假设，并得出结论：女生和男生的成绩存在显著

## Adjusted p-values 调整的p值
P 值校正就是为了校正假阳性，将假阳性的数目降低。
```R
x = c(1887.7, 1298.1, 1799.6)
y = c(800.2,  785.3,  800.4)
t = t.test(x,y)
print(t$p.value)
p_a <- p.adjust(t$p.value, n=2)
print(p_a)
--- 
[1] 0.04199959
[1] 0.08399918
```
调整后的 p 值大致为 `p * n`

尽管工具通常报告调整后（校正后）的 p 值，但在概念上，应调整显著性阈值。对于 `n=10` ，当过滤统计学显著性差异时，我们应该将 `0.05` 显著性水平降低到 `0.005` 。

## Multiple comparisons 多重比较
多重比较是指在进行多组实验或数据分析时，进行多个假设检验或对比的情况。当进行多个比较时，存在一定的概率会出现假阳性

每次检验的第一类错误概率是$\alpha$， 各次检验独立， 则检验$m$次， 在所有零假设都成立的情况下至少犯一次第一类错误的概率p为 $$p = 1-(1-\alpha)^m$$
举例来说 $m = 100$，$\alpha = 0.05$时， 至少犯一次第一类错误概率为 0.994。

修正 p 值时，通常使用的方法包括 [Bonferroni](https://en.wikipedia.org/wiki/Bonferroni_correction) 方法、Benjamini-Hochberg 方法。
```R
p = c (0.1, 0.2, 0.3, 0.4, 0.5)
p.adjust(p)
---
0.5  0.8   0.9   0.9   0.9
```

## False Discovery Rate 错误发现率
在所有拒绝零假设的情况中，实际上零假设为真的比例。例如，如果我们进行了 100 次检验，拒绝了 25 个零假设，并且其中有 5 个实际上是真的，那么错误发现率就是 5/25 = 0.2，即 20%。

在实际应用中，研究者通常会控制错误发现率在一定水平以下，例如 5%。这意味着，在所有拒绝零假设的情况中，不超过 5% 的情况是错误拒绝的。控制错误发现率有助于减少在多重假设检验中出现的误差，提高统计推断的准确性。

为了控制错误发现率，研究者通常采用一些方法来调整拒绝零假设的标准。常用的方法有贝叶斯因子（Bayes factors）、本质上的错误发现率（False Discovery Rate，FDR）和家族错误率（Familywise Error Rate，FWER）等。其中，FDR 是最常用的方法之一。

FDR 调整后的 p 值（FDR-adjusted p-values）是通过调整原始 p 值得到的，以便在整个检验过程中控制错误发现率。Benjamini-Hochberg（BH）步骤是计算 FDR 调整后 p 值的一种常见方法，其基本思想是对原始 p 值进行排序，然后使用排序后的 p 值和预先设定的错误发现率阈值来确定一个新的显著性水平。这个新的显著性水平用于拒绝零假设。

## Confidence intervals 置信区间
一种用于估计参数的区间估计方法。它表示对未知总体参数的估计范围，通过观测数据和抽样分布计算得出。置信区间通常包括一个中心点（例如样本均值）和一个范围，用于表示对总体参数（例如总体均值）估计的不确定性。

置信区间通常与置信水平（Confidence Level）一起使用，置信水平是对区间估计可信程度的度量。置信水平用百分比表示，例如 95% 置信水平表示在多次抽样和计算置信区间的过程中，大约有 95% 的置信区间将包含总体参数。


## Statistical robustness 统计稳健性
指统计方法在不同条件和假设下的稳定性和可靠性。具有统计稳健性的方法可以在数据中存在异常值或模型偏差的情况下，仍能够产生准确和可靠的结果。

在统计学中，通常有两种类型的稳健性：

1.  抗异常值稳健性（Robustness to outliers）：指统计方法对异常值或离群值的敏感性。具有抗异常值稳健性的方法可以在存在离群值时仍然提供可靠的估计。如中位数，因为它不受极端值的影响； 
2.  模型稳健性（Model robustness）：指统计方法对模型假设的敏感性。具有模型稳健性的方法可以在模型假设不完全符合实际情况时仍然提供可靠的估计，如加权最小二乘法（Weighted Least Squares）。

## 补充 - p value
> **P-values! Sixty percent of the time they work every time.**

[Interpreting P values](https://www.nature.com/articles/nmeth.4210)

Do I need to compute and discuss p-values? Yes, we use them because there is no better alternative.  