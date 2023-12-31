# 【bioinfo】-Q&A-38

## 第38题 当转录组普遍变化时RNA-Seq怎么进行分析(2)？
Hello 大家好！ 我们又见面了！

当出现这种所谓特殊情况的时候，可以使用Housekeeping gene的办法来进行相对定量，这种办法在一定程度上能够解决我们遇到的问题。但其实这种办法有一个非常强的先验假设：housekeeping gene的表达量不怎么发生变化。其实housekeeping gene list有几千个，这几千个基因有一定程度上的变化是有可能的。

有什么办法能够在RNA-Seq中对gene的定量起到“一锤定音”的作用呢？答案就是我们今天为大家介绍的spike-in方法，即在RNA-Seq建库的过程中掺入一些预先知道序列信息以及序列绝对数量的内参。这样在进行RNA-Seq测序的时候就可以通过不同样本之间内参（spike-in）的量来做一条标准曲线，就可以非常准确地对不同样本之间的表达量进行矫正。在这种操作下，可以一定程度上认为是一种绝对定量。

那么，有没有比较常用的spike-in类型呢？嘿嘿嘿～这是当然的啦！这个就是大名鼎鼎的ERCC Control RNA.

### 1. 什么是ERCC ?
ERCC = External RNA Controls Consortium

ERCC就是一个专门为了定制一套spike-in RNA而成立的组织，这个组织早在2003年的时候就已经宣告成立。主要的工作就是设计了一套非常好用的spike-in RNA，方便microarray，以及RNA-Seq进行内参定量。

ERCC 官方网站 https://www.nist.gov/programs-projects/external-rna-controls-consortium

### 2. ERCC control RNA具体是什么？
目前常用的ERCC版本内包含了176条RNA序列，这些RNA序列是从一些物种的DNA转录得到的，编号从ERCC-00001到ERCC-00176，所有对应的编号的序列全部可以从网上直接搜索下载。

目前，已经有非常成熟的商业化的ERCC序列，其中的176条RNA组成的一个库（pool）也是经过特殊设计的。不是简单的等量混合，而是不同的pool会对176条序列分成很多小库（subpool），然后通过不同的比例对这些subpool进行混合，然后形成不同的配比比例。在购买商业化的ERCC序列时，一定要注意下载对应版本的绝对定量信息。当然，有些商业化的ERCC里面也不是包含全部176条标准序列。

### 3. ERCC control在不同样本之间的表现是否良好？
目前的实验数据表明，ERCC spike-in的掺入效果还是非常不错的。这里引用了Thermo商品中的一张数据图。

![](1.jpg)
图3 ERCC绝对量与RPKM的关系 (https://www.thermofisher.com/order/catalog/product/4456740)

图3表明，在RNA-Seq中增加ERCC的绝对量是可以获得FPKM的绝对量的增加，并且两者成非常好的线性关系。这也是我们能够对RNA-Seq样本进行掺入内参的一个基本前提。

### 4. R里面可以使用哪个包来进行相关的操作？
说了这么多，那么如果真的有一套带有ERCC spike-in的RNA-Seq数据，我们应该怎么分析呢？其实在2014年的时候Nature Biotech上面发表了一篇文章，专门描述了怎么对数据进行分析，还顺手publish了一个R包，并且我认真阅读并实战过这个R包，从文档到结果都很靠谱。

这个R包的名称是 RUVSeq，链接为：

https://bioconductor.org/packages/release/bioc/html/RUVSeq.html


如果大家真的有需求，就直接follow这篇文章来进行学习就可以了。图5就是给大家展示一下使用ERCC spike-in矫正以后的结果。

![](2.jpg)
图5 矫正与不矫正基因表达量的boxplot

