# 【bioinfo】-Q&A-33

## 第33题 什么是链特异性的RNA-Seq？
Hello 大家好！我们又见面了。

最近，我们一直在围绕着RNA-Seq的相关技术在进行讨论，我们聊过了polyA+与rRNA-的建库策略问题，我们也聊过了测序深度与测序仪通量的问题，那么今天我们来聊一下链特异性RNA-Seq的建库问题。

### 1.什么是链特异性建库与链非特异性建库RNA-Seq
![](1.jpg)
图1-1 链特异性建库与非特异性建库示意图 （Zhao et al., BMC Genomics，2015）

在RNA-Seq建库的时候，第一步都是进行RNA到cDNA的反转录，在反转录以后，普通的RNA-Seq就直接使用random primer进行第2条链合成，随后加adapter。这样构建出来的RNA-Seq库进行测序以后是分不清这个序列是来自于genome的那条链的，因为被测序的有可能是gene的foward strand 也有可能是reverse strand。

而链特异性的RNA-Seq建库是通过一定的建库策略，让RNA在反转录和加adapter的过程以后还能够保存链的信息的建库策略。如图1-2所示，红色的reads表示mapping到了genome的foward strand，蓝色的reads表示mapping到了genome的reverse strand。对于这个gene来说，它本身存在于foward strand，链特意建库的F图就完全能够说明这个现象；但是G图是普通的RNA-Seq建库，也就是不区分链的信息，因此在这个foward strand gene附近的reads有mapping到foward strand的，也有mapping到reverse strand的情况。

![](2.jpg)
图1-2 F为链特异性建库，G为非特异性建库（Dmitri Parkhomchuk et al., NAR, 2009）

### 2.链特异性建库的优势与劣势
那么链特异性RNA-Seq的优势在于哪里呢？是在于它能够处理一些gene overlap比较复杂的情况。我们都知道，几乎所有高等生物的gene在genome中的分布都是非均匀的，而且一般都是没有链的偏好性。换句话说，两个gene重叠，一个在foward strand，一个在reverse strand是很常见的显现，如图2-1所示。

![](3.jpg)
图2-1 genome两条链都存在gene的情况（Joshua Z Levin et al., Nature Methods, 2010）

如果是普通的RNA-Seq，是分不清这些overlap区域的reads到底来自于哪一个gene，这就给定表达量带来了非常大的麻烦。但是链特异性的RNA-Seq就不会，如果只是foward strand的gene表达那么reads就只会mapping到对应的链上。

所以，用链特异性的建库方法，是能够更加准确进行gene定量的。

至于链特异性建库的劣势，大概有2点吧：1个是贵，1个是操作复杂对于珍贵样品（比如人体组织样品）一旦建库不成功就game over了。

### 3.链特异性建库的方法有几种，最常用的是哪种?
至于链特异性的建库方法，现在有若干种，但是最常用的其实是2种，1种是RNA ligation methods；1种是dUTP method。

#### 3.1 RNA ligation method
![](4.jpg)
图3-1 RNA ligation methods（Joshua Z Levin et al., Nature Methods, 2010）

这种方法的本质上就是对mRNA先加上adatper再进行反转录，这样所有的就能保存序列信息都是5’ adapter -> gene -> 3’ adapter的顺序。因此能够进行链特异性建库。

#### 3.2 dUTP methods

![](5.jpg)
图3-2 dUTP method （Joshua Z Levin et al., Nature Methods, 2010）

这种方法是在合成第2条cDNA链的时候，把dTTP换成dUTP，这样DNA中原来T的位置就全都变成了U，随后两边加上adapter序列。到这里，是存在两种情况的。
```
第1种情况：5' adapter -> gene -> 3' adapter
第2种情况：5' adapter -> gene 反向互补 -> 3' adapter
```
但是，无论是这2中情况中的哪一种，gene的反相互链中都原来的T都被替换成了U，这时候我们使用USER酶就可以特异性降解带有U的那条链。因此这时候就只能保存第1种情况了。因此也就可以进行链特异性的测序。

### 4.提问环节
4.1 第1个问题是我刚学生信的时候的一个疑问， 对于普通的RNA-Seq建库来说，衡量gene表达量，对于foward gene来说，如果有互补链的reads，在计算FPKM的时候需不需要把互补链的reads都去掉？如果是链特异性的RNA-Seq建库呢？为什么？

>个人认为，如果是普通的RNA-seq的建库，它没有区分正链负链是无法把互补链的reads去掉的。因为分不清楚，而且在进行FPKM计算的时候，结果可能会偏高！如果是链特异的RNA-seq建库的话，肯定是需要去掉互补链的。两个gene重叠，一个在foward strand，一个在reverse strand是很常见的显现，在计算的时候，算一条链就行了。

4.2 第2个问题，请结合我们的Bioconductor教程来进行。请尝试分析human genome gene 的overlap的情况。

```python
library(GenomicRanges) ## 包的导入
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install("EnsDb.Hsapiens.v75", version = "3.8") ###到这一步是进行人类参考基因组的下载
library(EnsDb.Hsapiens.v75 ) ##参考基因组的导入
ensembl.hg38 =EnsDb.Hsapiens.v7 ##赋值一下
ensembl.hg38.gene=genes(ensembl.hg38)##进行把人类基因取出来
length(ensembl.hg38.gene)##看一下长度
[1] 64102
reduce(ensembl.hg38.gene) ##利用reduce函数看一下结果
length(reduce(ensembl.hg38.gene))
[1] 53110
```

