# 【bioinfo】-Q&A-32

## 第32题 1个RNA-Seq样本到底要测多少序列？


我们讨论了到底选择哪种策略进行建库，主要就是polyA + 和 rRNA-这两种方式。在选定建库策略以后，我们自然会问一个问题，如果我们去做RNA-Seq每个重复到底要测多少测序量呢？

### 先聊聊测序价格

我们这里讨论的问题，都是以Illumina公司的二代测序为准。目前市面上公司能够提供测序服务的，大多数是Illumina的HiSeq平台，HiSeq X平台，NextSeq平台以及通量最高的NovaSeq平台。以上这些平台，在平时实验中我们都是进行双端150bp的测序模式，也就是我们常说的paired-end 150bp，所以一对reads的通量是150bp X 2 = 300bp

注：下问中提到所有测序仪参数全部来自于Illumina官网！

#### 1.1 HiSeq系列

HiSeq系列的参数如图1-1所示，最常用的是HiSeq2500与HiSeq4000。HiSeq系列目前已经逐渐被边缘化或者是定制化，主要是用来做定制测序双端 250 bp的测序，或者单端50bp的测序。根据定制的模式不同，价格也不同。平均下来，价格大约是100~ 120 RMB / 1Gbp

![](1.jpg)
图1-1 HiSeq系列平台参数

#### 1.2 HiSeq X Ten测序仪

这个型号的测序仪是目前市场上的主力，1个flowcell可以有8条lane，每条lane可以产生100~120Gbp的测序数据，包lane的价格大约在8000RMB ~ 10000RMB左右，平均下来的价格是80 ~ 100RMB / Gbp

![](2.jpg)
图1-2 Illumina HiSeq X Ten测序仪

#### 1.3 测序最快的NextSeq系列

NextSeq采用了最新的双色荧光系统，而不是Illumina以前的四色荧光系统，因此运行速度大大提升，大约1天左右的时间就可以完成上机测序（HiSeq大约是3到4天），这个测序仪只有在我们对样本和实验比较着急的时候才会使用。价格也会比HiSeq系列高若干倍。不过，NextSeq的出现，给我们提了个醒，Illumina以后会通过逐渐优化测序仪站稳医院临床快速检测的脚跟。

#### 1.4 通量最高，最便宜的NovaSeq系列

NovaSeq系列测序仪没有lane的概念，是一个完整的flowcell，运行一次可以产生6Tbp左右的数据。如果你想测个几百个样品，那么NovaSeq绝对是你的首选。现在市场价大约可以压缩到40~50RMB/Gbp

### 对于human来说RNA-Seq需要测多深？
这个问题之前已经有人专门研究过，实验方案就是先把样品测很深比如50M pair（1M = 10^6），然后去随机抽取若干序列，比如每隔10M做一次抽样，再去计算每一次抽样结果的FPKM值，最后看基因的FPKM在多少测序量的时候能够稳定。根据图2-1所示，大约到了30M reads 的时候就可以稳定找出差异表达基因。
![](3.jpg)
图2-1 探索测序量与差异表达基因的关系 （Yuwen Liu et al., 2014, Bioinformatics）

同时，ENCODE计划里面也公布了RNA-Seq的数据测序标准，在ENCODE的标准中原文如下：

>\1. Replicate number: Experiments should be performed with two or more
biological replicates.      
>\2. Sequencing depth: 30M pair-end reads of length > 30NT, of which 20-25M are mappable to the genome or known transcriptome.

简单翻译一下，就是要求生物学重复不少于2个，如果是估算表达量的情况下30M paried end reads就差不多了，同时最好能够保证其中的20~25M的paried reads都能回贴到参考转录组中。

### 提问环节
**3.1 对于human来说，genome大约为3Gbp，可以转录的区域大约是30%左右的genome，但是gene的exon只有不到whole genome的10%，在这种情况下我们需要测30M paired-end reads才可以比较好的衡量差异表达基因。如果换一个物种，我们又该怎么去判断如何定测序量呢？**

![](4.png)

3.2 算算账吧，如果一个样本测**30M paired-end reads**，那么以目前的市场价格，测序需要花多少钱？还可以算算一个常规的RNA-Seq实验的测序经费是多少？

但是因为还需要生物学重复。对于临床的样本和普通的细胞样本来说的话是不一样的。    
如果是普通的细胞系的话3-4个重复，咱们按3个重复计算一个样品的话是540x3=1620RMB 。但是对于临床的肿瘤样品来说的话，是需要6-8个重复（因为肿瘤的差异比较大）。按6个计算，一般要一个肿瘤样本，一个癌旁样本，这样就是540x6x2=6480 RMB

