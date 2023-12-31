# 【bioinfo】-Q&A-35

## 第35题 RNA-Seq 数据的定量之RPKM和FPKM
Hello大家好！好久不见了！

根据之前的规划，我们将用接下来的几期问题来探索一下RNA-Seq定量的问题，也就是要探索一下我们常说的RPKM，FPKM，TPM，raw count 和RSEM，前面4个指标都比较直观，方便理解，最后一个RSEM需要涉及到一些机器学习的知识，我们尽量给大家把比较复杂的问题简单化，方便大家的入门。

### RNA-Seq定量过程中的比较问题
RNA-Seq的基本假设是什么？简单来说就是 细胞/组织/个体 的两种不同状态进行比较，比较的目的就是寻找差异表达gene，然后从差异表达gene来推断造成生理状态不同的原因。

而我们的RNA-Seq一般情况下是针对mRNA以及带polyA的lncRNA进行建库测序分析的。那么理论上把测序的FASTQ文件mapping到参考基因组上，再结合参考基因组的GTF/GFF文件就可以找到全基因组的每一个gene上mapping到了多少个reads count。

拿到了reads count以后，我们就会尝试着想要比较gene之间的表达量的关系，但是这时候往往会面临两个问题，举个例子：

* 问题1: 比如我有gene3，有1000条测序reads，gene4有2000条测序reads，那么我能否说gene4就一定比gene3的表达量高？（图1 gene3 与 gene4）
* 问题2: 比如我有gene1，有1000条测序reads，我的另一个处理条件下gene2有2000条测序reads，我能否就说geneA在处理条件下表达量降低了？（图1 gene1与gene2）

在面临这些比较问题的时候，我们就需要对mapping到gene的reads count进行矫正，至少根据问题1我们知道应该在矫正的时候考虑过gene长度的问题；根据问题2，我们大概应该能够猜想到，矫正的时候应该需要考虑整体测序量的问题。到此，RPKM和FPKM这两个指标就应运而生了。

![](1.jpg)
图1 ( Manuel Garber et al., Nature Methods, 2011 )

### 什么是RPKM与FPKM？
RPKM = Reads Per Kilobase per Million mapped reads

假设回贴到geneA 的 reads count为 CountA，geneA的exon总长度为Len(A) Kbp，总的测序量为D兆reads，那么：
```
geneA RPKM = CountA / Len(A) / D * 10^9
```
那么什么是FPKM呢？先来看一下FPKM的定义：

FPKM = Fragments Per Kilobase per Million mapped reads

大家可以比较清楚看出来，RPKM中的R指的是Reads，FPKM中的F是指Fragments，Reads都比较好理解，就是我们的测序短的片段，那么fragment是什么呢？这是以为我们现在测序一般来说都是测双端测序（paired-end sequencing），那么在mapping回参考基因组的时候就会有两条reads，分别是read1和read2，分别来源于建库打断的5’ 端和3’端。那么这2条reads就可以在参考基因组上确定1个小的片段，这个片段就叫fragment（图2所示）。

![](2.jpg)
图2 （Frances S. Turner）

所以，如果是现在最常用的双端测序，1个gene的FPKM应该等于RPKM / 2。

###  RPKM / FPKM有什么优缺点？
因为现在使用Illumina测序平台，绝大多数的测序都是使用双端测序，那么基本上我们一般对gene进行定量都是使用FPKM来进行。FPKM的优点大家都很了解了，能够矫正掉**gene长度以及测序深度**对gene表达定量的影响，那么FPKM的缺点大家是否熟悉呢？

一个比较容易被人提及的问题是对于不同批次测序的结果，所有gene的FPKM的总和不是一个固定的值。比如WT测的所有gene的FPKM总和可能是10000，treat组测到的FPKM总和可能是15000，这样对于WT和treat组之间的差异表达gene的寻找就有可能出现问题，这个时候就需要用到我们常用的另一种矫正方法TPM。

