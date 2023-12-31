# 【bioinfo】-Q&A-48

## PRC复合物抑制/激活的基因干扰了单细胞的基因表达
> Flipping between Polycomb repressed and active transcriptional states introduces noise in gene expression

#### 背景知识
Polycomb repressive complexes (PRCs) 是非常著名的组蛋白修饰复合物，一般来说是抑制基因表达的。但是如果某基因同时被PRC复合物和RNA polymerase II (RNAPII)结合，也可以被激活转录。但其中机理不明。

- PRC1, which monoubiquitinylates histone 2 A lysine 119 (H2Aub1) via the ubiquitin ligase RING1A/B;
- PRC2, which catalyzes dimethylation and trimethylation of H3K27 (H3K27me2/3) via the histone methyltransferase (HMT) EZH1/2.

Embryonic stem cells (ESCs) 能自我更新并且具有分化成其它细胞类型的潜力，并且认为其干细胞特性由表观调控保持。一般经由干细胞marker挑选，比如Oct4，需要很明确的证明其不表达分化marker，比如Gata4 and Gata6。可以对这两个marker基因集合做热图，如下：






**RNAPII的修饰**决定着转录过程，主要是其碳末端的磷酸化修饰。具体已知结论如下：
```
Phosphorylation of S5 residues (S5p) correlates with initiation, capping, and H3K4 HMT recruitment.
S2 phosphorylation (S2p) correlates with elongation, splicing, polyadenylation, and H3K36 HMT recruitment.
Phosphorylation of RNAPII on S5, but not on S2, is associated with Polycomb repression and poised transcription factories, while active factories are associated with phosphorylation on both residues.
S7 phosphorylation (S7p) marks the transition between S5p and S2p, but its mechanistic role is unclear presently.
```
如果把PRC复合物和RNAPII的修饰结合起来，可以把基因分成两类：
```
(1) repressed genes associated with PRCs and unproductive RNAPII (phosphorylated at S5 but lacking S2p; PRC-repressed)
(2) expressed genes bound by PRCs and active RNAPII (both S5p and S2p; PRC-active)
```
当然，这些基因都被H3K4me3 and H3K27me3共同结合，被称作二价状态。

#### 单细胞转录组测序
测的是血清+ leukemia-inhibitory factor (LIF)培养的小鼠OS25 ESCs 细胞，用的是 Fluidigm C1进行单细胞获取，建库用的是SMARTer试剂盒。

单细胞过滤：

(1) the total number of reads mapping to exons for the cell was lower than half a million
(2) the percentage of reads mapping to mitochondrial-encoded RNAs was higher than 10%.
最后剩下90个单细胞进入后续分析，这些细胞都超过80%的比对率，而且超过60%的reads是落在外显子区域的。

基因过滤：

过滤那些RPM小于10的低表达量基因，因为实在是没有办法区分它们的生物学差异和技术差异。

把单细胞表达矩阵的基因平均表达量跟Brookes et al.5 的bulk转录组测序数据结果比较，相关性非常好。

#### 分析 Cell-to-cell variation
细胞之间的基因表达差异来自于3大因素：
```
tochastic gene expression itself
technical noise
confounding expression heterogeneity due to biological processes such as the cell cycle.
```
作者首先重构这些细胞的细胞周期状态，分析到细胞周期对细胞之间的表达差异贡献才1.2% ，并且矫正该影响。

为了矫正技术误差，作者去除了那些低表达量基因，平均reads数小于10的那些，最后剩下一万一多基因。

**最后作者用DM (distance to median)来衡量基因在细胞群里的表达变异情况。**

DM这个指标非常给力，超脱了基因长度以及基因表达量的限制，如下：






#### 结合公共ChIP-seq 数据
分析公共数据 GSE34520 ，把基因根据 PRC marks and RNAPII states进行分类
```
(1) “Active” genes (n = 4483) without PRC marks (H3K27me3 or H2Aub1) but with active RNAPII (S5pS7pS2p) 这些基因大多数管家基因，所以表达量较为稳定
(2) “PRC-active” genes (labeled as “PRCa”; n = 945) with PRC marks (H3K27me3 or H3K27me3 plus H2Aub1), and active RNAPII.这些基因大多数信号通路
(3) “PRCr” genes (n = 954) have both PRC marks (H3K27me3 and H2Aub1), unproductive RNAPII (S5p only and not recognized by antibody 8WG16) and not expressed in bulk mRNA data by Brookes et al. (bulk mRNA FPKM <1).
```
经由 two-tailed Wilcoxon rank sum 检验，发现 “PRC-active” 基因集 统计学显著的在单细胞水平有着更高的表达变异程度，相比 “Active” genes 。

数据包括：

GSM850467RNAPII S5P ChIPSeqGSM850468RNAPII S7P ChIPSeqGSM850469RNAPII 8WG16 ChIPSeqGSM850470RNAPII S2P ChIPSeqGSM850471H2Aub1 ChIPSeqGSM850472H3K36me3 ChIPSeqGSM850473Control MockIPGSM850474Ring1B ChIPSeqGSM850475RNAPII S5P Repeat ChIPSeqGSM850476OS25 cells mRNA-Seq


#### 结合公共单细胞转录组数据
Single cell RNA-sequencing of pluripotent states unlocks modular transcriptional variation. Cell Stem Cell 17, 471–485.

#### 结合Hi-C的3D基因组数据
有一个公共数据：GOTHiC (Genome Organization Through Hi-C) Bioconductor package .

参考文献：Schoenfelder, S. et al. Polycomb repressive complex PRC1 spatially constrains the mouse embryonic stem cell genome. Nat. Genet. 47, 1179–1186 (2015).

#### 定义某个基因是否是某些组蛋白修饰marker的阳性
Genes were defined as positive for H3K9me3 at their promoter or gene body when an enriched region was overlapping with a 2 kb window around the TSS or between the TSS and TES, respectively.



#### 基因表达差异的衡量
Gene expression variation can be quantified by CV or DM, which is a measure of noise independent of gene expression levels and gene length.

#### coefficient of variation (CV)
衡量基因在某个细胞群体里面的表达差异，这个CV应用最广泛了，但它被基因长度和基因的表达量影响。是概率分布离散程度的一个归一化量度，其定义为标准差 与平均值 之比： 变异系数（coefficient of variation）只在平均值不为零时有定义，而且一般适用于平均值大于零的情况。

#### DM (distance to median)
首先计算 a mean corrected residual of variation by calculating the difference between the observed squared CV (log10-transformed) of a gene and its expected squared CV.

然后 correct for the effect of gene length on the mean corrected residual of variation

这个计算得到的the mean corrected residual of the gene 和 its expected residual 的差异就是 DM

根据 DM排序后可以来定义： top 20% as “noisy” genes and the bottom 20% as “stable” genes.

The expected squared CV or the expected residual was approximated by using a running median.

这个计算公式参考： https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4595712/#mmc1

#### 单细胞表达量poisson-beta分布模型
参考自文献；Beta-Poisson model for single-cell RNA-seq data analyses

#### burst frequencies
The beta-Poisson model captures the burst frequency and burst size through the shape and scale parameters α and β, respectively. Large α indicates high burst frequency; large β means large burst size

#### 使用 scLVM 去除细胞周期对表达量的影响
Removing cell cycle variation and technical noise allowed us to focus on stochastic gene expression.


数据分析结果解读


很明显，active系列的基因 和PRCa系列基因在各个指标上面有统计学显著的差异。

最后还要一堆实验验证，我就懒得看了。

