# 【bioinfo】-Q&A-25

## 第25题 GTF/GFF的注释是怎么来的，应该从哪里下载？
Hello everyone! 我们又见面了！

我们昨天刚刚回答了GTF/GFF文件到底是什么的问题，那么我们今天尝试回答GTF/GFF文件是怎么来的，从哪里能够下载。

根据BBQ24问的介绍，我们知道了**GTF是GFF的2.0版本**。（以后我们把这两者到简称为GTF文件好了！）主要记录的内容是基因组上基因的结构，包括哪里是exon，哪里是intron，哪里是CDS，哪里是start codon，哪里是stop codon. 在提问的环节，我们也设置了问题让大家去吐槽GTF文件糟糕的的文件结构（希望大家能吐得开心！）。

今天我们来为大家介绍GTF文件是怎么来的。

### 1. 什么是参考GTF/GFF文件
针对一些已经通过测序计划，拼装好基因组序列信息的物种，一般会同时提供其转录组的注释信息，也就是我们所说的GTF/GFF文件。常用的模式生物，比如human（人），mouse（小鼠），rat（大鼠），chicken（鸡），lizard（蜥蜴），Arabidopsis thaliana（拟南芥）等等都已经有非常好的 全基因组参考序列（FASTA文件），以及转录组注释信息（GTF或GFF文件）。

因此，直接到能够提供下载地址的网站上下载就好了。

图1给大家展示了已经公布参考基因组哺乳动物的系统发生树，大家可以看看人类和哪种动物演化距离最近。

![](../../../../../Desktop/md/【bioinfo】-Q-A-25/1.jpg)
图1 已经公布参考基因组的哺乳动物系统发生树（http://asia.ensembl.org/info/about/speciestree.html）

多说一句，这个下载下来的FASTA文件就是我们所谓的参考基因组，**需要用这个文件去构建mapping的index；下载下来的GTF文件是转录组注释信息，一般在计算表达量的时候需要提供。**

### 2. 什么是拼装转录本
在进行转录组分析的过程中，我们经常会听到一句话叫“用XX软件拼装转录本”，这句话是什么意思呢？不都有参考转录组了，还要拼个啥转录组？

第1个方面，对于**无参考基因组，无参考转录组的物种**来说，往往需要通过RNA-Seq的数据自己拼出参考转录组，然后再进行下游的数据分析。所以，对于无参分析来说，往往需要自己拼装转录本，生成自己的参考转录组信息，也包括注释信息（GTF文件）。

第2个方面，对于**有非常好注释的基因组**，例如human来说。不同的细胞条件可能不同，一些永生化的细胞系往往都具有“癌症”的特征，这些细胞的转录组，基因组或多或少都有结构的变异，以及转录本的差异。

举个例子，有一个geneA，在参考转录组中注释的是从chr1:1000 ~ 15000进行转录，但是在另外的细胞系中有可能就是从chr1:980 ~ 15050进行转录。这种转录起始和终点的不同是很常见的现象。

**因此，有时候为了比较严谨地进行下游序列分析，是需要根据已知的参考转录组以及测序数据对其进行一个修饰。**

常用的软件是cufflinks，stringtie等等。

不过，对于有参考转录组的物种，一般情况下，我还是建议不要去自己拼转录本，也不要去做什么所谓的修正，意义不大。除非你研究的体系非常特殊。

### 3. 从哪里下载参考转录组GTF/GFF文件呢？
关于下载GTF/GFF文件的内容，给大家介绍2个最常用的网站：

* 1个是UCSC genome browser ([UCSC Genome Browser-网址链接](https://genome.ucsc.edu/index.html)）
* 1个是Ensembl（[Ensembl 网址链接](http://asia.ensembl.org/index.html)）
* 注意：对于植物的Ensembl网站（Ensembl Plants）

#### 3.1 从UCSC genome browser下载human的GTF文件
```
1. 打开UCSC genome browser网站
2. 在Tools里选择 Table Browser
3. 打开Table Browser以后，设置相关的需要内容（图3.1）
4. 点击get output即可下载
# hg19 = human genome 19是常用的human参考基因组版本号；
# RefSeq gene是全部经过人工检查过的gene注释文件；
```
![](../../../../../Desktop/md/【bioinfo】-Q-A-25/2.jpg)

图3.1 打开Table Browser以后，设置相关的需要内容

#### 3.2 Ensembl下载human的GTF文件

在下载之前我必须跟大家提个醒。
```
* 对于动物相关的信息都请访问Ensembl的动物站：http://www.ensembl.org/index.html
* 对于植物相关的信息都请访问Ensembl的植物站：http://plants.ensembl.org/index.html
```
我们在这里还是以下载human hg19版本的GTF文件为例，操作步骤如下：

【博主用的全是最新的CRCh38】
```
1. 登陆Ensembl网站，并跳转到hg19版本界面
2. 继续选择跳转到hg19版本界面
3. 在hg19版本的Ensembl界面中选择download
4. 在download页面中选择Download a sequence or region
5. 在左边栏选择 FTP download 然后选择下载 GTF文件
6. 选择注释好的GTF进行下载
```

### 4. 提问环节

**1. 请按照文中教程分别从UCSC Genome Browser，以及Ensembl网站上下载hg19的转录组注释的GTF格式文件。**
![](../../../../../Desktop/md/【bioinfo】-Q-A-25/3.png)

**2. 下载这两个文件解压缩以后的大小是否有差异，差异大不大？**
```
两个网站下载的GTF⽂件大小差异较大，解压之后
hg19_RefSeq_GTF_UCSC⽂件126M，
Homo_sapiens.GRCh37.87.chr.gtf⽂件大小是1.2G。
```
**3. 解压并使用Linux less命令打开这两个文件，观察这两个文件的transcript_id以及gene_id是否相同，再找找看有哪些其他地方的不同。**
```
两个⽂件的transcript_id以及gene_id均不相同，Ensembl网站下载的gtf⽂文件transcript_id以及 gene_id均以ENSG和ENSG开头，全称是Ensembl Transcript ID和Ensembl Gene ID，而UCSC网站下载的gtf文件transcript_id以及gene_id均以NM开头,在NCBI数据库中代表mRNA;

除此之外，UCSC 的注释信息非常简练，⽽Ensemble⽹站中的gtf文件注释信息相比于UCSC更加全面，但也存在冗余，NCBI 数据库中的基因注释被验证的比例更大，

所以在⽐对策略上可以选择先使用UCSC的GTF文件筛选目标基因，
再利用Ensemble数据库的GTF⽂件找到更加详细的注释。
```
```python
# Ensembl gtf文件:
1 havana exon 12613 12721 . + . gene_id "ENSG00000 223972"; gene_version "4";	transcript_id "ENST00000456328"; transcript_version "2" ; exon_number "2"; gene_name "DDX11L1"; gene_source "ensembl_havana"; gene_biotype	"pseudogene"; transcript_name "DDX11L1-002"; transcript_source "havana"; transcri pt_biotype	"processed_transcript"; havana_transcript "OTTHUMT00000362751"; havana_	transcript_version "1"; exon_id "ENSE00003582793"; exon_version "1"; tag "basic";

# UCSC gtf⽂件:
chr1 hg19_ncbiRefSeq exon 66999929 67000051 0.000000 +	. gene_id "NM_001308203.1"; transcript_id "NM_001308203.1";
```

