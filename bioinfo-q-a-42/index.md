# 【bioinfo】-Q&A-42

## 用米氏方程解决单细胞转录组dropout现象

>米氏方程（Michaelis-Menten equation）: v=Vmax × [S] /（Km+[S])
在假定存在一个稳态反应条件下推导出来的，其中 Km 值称为米氏常数，Vmax是酶被底物饱和时的反应速度，[S]为底物浓度。
Km值的物理意义为反应速度（v）达到1/2Vmax时的底物浓度（即Km=[S]），单位一般为mol/L，只由酶的性质决定，而与酶的浓度无关。可用Km的值鉴别不同的酶。

今天要介绍的这篇文章提出了一个算法，R包是：M3Drop ， 文章是：Modelling dropouts for feature selection in scRNASeq experiments

### 挑选重要基因
目前已有的寻找单细胞转录组测序数据中的重要基因（feature selection）的方法都不够好，比如 scLVM 主要是根据先验基因集，比如cell-cycle or apoptosis来区分细胞。与此相反，基于 highly variable genes (HVG) 的方法挑选到的变化量大的那些基因很可能是技术带来的误差。而且低表达量基因的变动往往大于高表达量基因，而且所谓的表达变化大也并没有很好的生物学解释。 一个比较好理解的概念是差异基因，但是需要预先把细胞群体分组后进行比较才能得到，而很多时候细胞太相似了，没办法很好的分开。像PCA或者t-SNE这样的降维方法也可以用来挑选重要基因，但它们也受制于系统误差或者批次误差等等。 dropout是scRNASeq数据的一大特点，就是很多基因在某些细胞根本就不表达，但是在另外的细胞却高表达。这篇文章作者对全长转录本数据和基于UMI的表达量数据分别提出了对应的解决方案，Michaelis-Menten equation 和 depth adjusted negative binomial (DANB) 。

单细胞转录组数据里面的dropouts可以达到50%，但是通常认为这个dropouts是因为在文库构建的过程中，有部分基因没有被成功的反转录，是一个酶促反应。 所以作者用Michaelis-Menten 来建模。

### 比较9种 feature selection 方法
使用它们分别对基因排序，算法如下：
```
by the magnitude of their loadings in principal component analysis (PCA)
by the strength of their most negative gene-gene correlation (Cor)
by their relative Gini index (Gini)
M3Drop dropouts-mean expression curve (M3Drop)
the squared coefficient of variation (CV2)
mean expression relationship (HVG)
the dispersion-mean expression relationship fit by DANB (NBDisp)
the dropouts-mean expression relationship fit by DANB (NBDrop).
```
这些算法都不需要预先对样本进行分类，是无监督的算法。
```
differentially variable (DV)genes
highly variable (HV) genes
differentially expressed (DE) genes
```
单细胞转录组数据的batch effects比较严重，所以 feature selection 过程的一个主要目的就是降低技术误差的影响，集中在有生物学意义的差异上面。

### 公共数据集
作者比较了 5个公共数据集，都是小鼠的胚胎细胞，含有17~255个细胞的测序数据，包括zygote to blastocyst.
```
Tung et al. (2017) [12] considered iPSCs from three different individuals and performed three replicates of UMI-tagged scRNASeq and three replicates of bulk RNASeq for each. (GSE77288 ).
For Kolodziejczyk et al. (2015)，we considered ESCs grown under two conditions: alternative 2i and serum for which there were three replicates of scRNASeq and two replicates of bulk RNASeq.( E-MTAB-2600 )
```
对bulk转录组数据用了3种方法找差异基因，分别是 DESeq2,edgeR,limma-voom，只有3种方法都是 5% FDR的差异基因才认为是阳性标准基因集，那些3种方法都在 20% FDR的非差异基因认为是阴性金标准。
```
1,915 positives, and 8,398 negatives for the iPSCs
709 positives and 11,278 negatives for the ESCs
有了这些基因，就可以计算ROC
```
细胞转录组数据文章一般分成下面两大类：

第一类是：deep sequencing of full-transcripts for a relatively small number of cells

第二类是：high-cell number, low-depth sequencing of 3’ or 5’ ends of transcripts tagged with unique molecular identifiers

