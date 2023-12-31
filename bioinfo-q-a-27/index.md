# 【bioinfo】-Q&A-27

## 第27题 GENCODE与Ensembl GTF/GFF到底哪里不同？怎么下载？

### 1. 基因注释的不同版本问题


* RefSeq Gene注释，对gene的不同转录本进行注释，1个转录本对应1个编号成为RefSeq id，例如对于可以翻译成蛋白的转录本，都会以NM_开头如NM_015658；对于不能翻译的转录本，都会以NR_开头如NR_027055；
* **Ensembl注释；**对gene的不同转录本进行注释，以ENSG开头的表示Ensembl gene_id如ENSG00000227232，以ENST开头的表示Ensembl transcript id如ENST00000438504
* **UCSC gene注释；**对gene的不同转录本进行注释，一般是类似uc004cpf这样的名称。

但是，RefSeq有一些注释不够完整，或者说有一些可能存在的，有用的gene转录本，所以有的时候我们会推荐大家使用Ensembl或者GENCODE的注释，那么我们今天的问题来了——Ensembl和GENCODE的GTF/GFF注释有什么不同？

### 2. GENCODE上下载GTF/GFF
先说说GENCODE是什么吧，根据GENCODE的官方说明文档，GENCODE的目的是为了建立一个公用可信的gene注释体系，其缩写的对应关系为：

**The GENCODE Project = Encyclopædia of genes and gene variants**

好了，让我们先访问以下GENCODE的官方网站：https://www.gencodegenes.org/

其实，大家可以发现GENCODE的网站上，只提供了Human和Mouse两个物种的注释信息，但这两个物种也是平时研究的时候最关心最常用的两个物种。每个物种有若干个版本，我们一般下载最新版本就好了。下面以Human的数据下载为例为大家进行讲解。

大家可以看出来，GENCODE已经对数据做了非常好的整理，不但提供了各种类型数据的下载，而且在下方的Metadata部分，还解释了每一种数据的元数据。也就是不但告诉我们注释的结果，还告诉了我们为什么这么注释，真的很良心！

在这里，我们就为大家下载了最常用的gene annotation也就是第1个文件Comprehensive gene annotation 的 GTF格式。

### 3. GENCODE与Ensembl GTF/GFF有什么不同？
我们先来看一下这两者的数据内容：

从Ensembl下载下来的GTF文件；
```
#!genome-build GRCh37.p13
#!genome-version GRCh37
#!genome-date 2009-02
#!genome-build-accession NCBI:GCA_000001405.14
#!genebuild-last-updated 2013-09
1       ensembl_havana  gene    11869   14412   .       +       .       gene_id "ENSG00000223972"; gene_version "4"; gene_name "DDX11L1"; gene_source "ensembl_havana"; gene_biotype "pseudogene";
1       havana  transcript      11869   14409   .       +       .       gene_id "ENSG00000223972"; gene_version "4"; transcript_id "ENST00000456328"; transcript_version "2"; gene_name "DDX11L1"; gene_source "ensembl_havana"; gene_biotype "pseudogene"; transcript_name "DDX11L1-002"; transcript_source "havana"; transcript_biotype "processed_transcript"; havana_transcript "OTTHUMT00000362751"; havana_transcript_version "1"; tag "basic";
1       havana  exon    11869   12227   .       +       .       gene_id "ENSG00000223972"; gene_version "4"; transcript_id "ENST00000456328"; transcript_version "2"; exon_number "1"; gene_name "DDX11L1"; gene_source "ensembl_havana"; gene_biotype "pseudogene"; transcript_name "DDX11L1-002"; transcript_source "havana"; transcript_biotype "processed_transcript"; havana_transcript "OTTHUMT00000362751"; havana_transcript_version "1"; exon_id "ENSE00002234944"; exon_version "1"; tag "basic";
1       havana  exon    12613   12721   .       +       .       gene_id "ENSG00000223972"; gene_version "4"; transcript_id "ENST00000456328"; transcript_version "2"; exon_number "2"; gene_name "DDX11L1"; gene_source "ensembl_havana"; gene_biotype "pseudogene"; transcript_name "DDX11L1-002"; transcript_source "havana"; transcript_biotype "processed_transcript"; havana_transcript "OTTHUMT00000362751"; havana_transcript_version "1"; exon_id "ENSE00003582793"; exon_version "1"; tag "basic";
1       havana  exon    13221   14409   .       +       .       gene_id "ENSG00000223972"; gene_version "4"; transcript_id "ENST00000456328"; transcript_version "2"; exon_number "3"; gene_name "DDX11L1"; gene_source "ensembl_havana"; gene_biotype "pseudogene"; transcript_name "DDX11L1-002"; transcript_source "havana"; transcript_biotype "processed_transcript"; havana_transcript "OTTHUMT00000362751"; havana_transcript_version "1"; exon_id "ENSE00002312635"; exon_version "1"; tag "basic";
```
刚刚下载下来的GENCODE的GTF文件；
```
##description: evidence-based annotation of the human genome (GRCh38), version 28 (Ensembl 92)
##provider: GENCODE
##contact: gencode-help@ebi.ac.uk
##format: gtf
##date: 2018-03-23
chr1    HAVANA  gene    11869   14409   .       +       .       gene_id "ENSG00000223972.5"; gene_type "transcribed_unprocessed_pseudogene"; gene_name "DDX11L1"; level 2; havana_gene "OTTHUMG00000000961.2";
chr1    HAVANA  transcript      11869   14409   .       +       .       gene_id "ENSG00000223972.5"; transcript_id "ENST00000456328.2"; gene_type "transcribed_unprocessed_pseudogene"; gene_name "DDX11L1"; transcript_type "processed_transcript"; transcript_name "RP11-34P13.1-002"; level 2; transcript_support_level "1"; tag "basic"; havana_gene "OTTHUMG00000000961.2"; havana_transcript "OTTHUMT00000362751.1";
chr1    HAVANA  exon    11869   12227   .       +       .       gene_id "ENSG00000223972.5"; transcript_id "ENST00000456328.2"; gene_type "transcribed_unprocessed_pseudogene"; gene_name "DDX11L1"; transcript_type "processed_transcript"; transcript_name "RP11-34P13.1-002"; exon_number 1; exon_id "ENSE00002234944.1"; level 2; transcript_support_level "1"; tag "basic"; havana_gene "OTTHUMG00000000961.2"; havana_transcript "OTTHUMT00000362751.1";
chr1    HAVANA  exon    12613   12721   .       +       .       gene_id "ENSG00000223972.5"; transcript_id "ENST00000456328.2"; gene_type "transcribed_unprocessed_pseudogene"; gene_name "DDX11L1"; transcript_type "processed_transcript"; transcript_name "RP11-34P13.1-002"; exon_number 2; exon_id "ENSE00003582793.1"; level 2; transcript_support_level "1"; tag "basic"; havana_gene "OTTHUMG00000000961.2"; havana_transcript "OTTHUMT00000362751.1";
chr1    HAVANA  exon    13221   14409   .       +       .       gene_id "ENSG00000223972.5"; transcript_id "ENST00000456328.2"; gene_type "transcribed_unprocessed_pseudogene"; gene_name "DDX11L1"; transcript_type "processed_transcript"; transcript_name "RP11-34P13.1-002"; exon_number 3; exon_id "ENSE00002312635.1"; level 2; transcript_support_level "1"; tag "basic"; havana_gene "OTTHUMG00000000961.2"; havana_transcript "OTTHUMT00000362751.1";
```
仔细观察一下，我们发现两者貌似没什么不同，除了GTF第9列的可选信息写的顺序不一样，不过这都没有什么影响。我们来看看官方是怎么回答这个问题的：

>What is the difference between GENCODE and Ensembl annotation?
The GENCODE annotation is made by merging the Havana manual gene annotation and the Ensembl automated gene annotation. The GENCODE annotation is the default gene annotation displayed in the Ensembl browser. The GENCODE releases coincide with the Ensembl releases, although we can skip an Ensembl release is there is no update to the annotation with respect to the previous release. In practical terms, the GENCODE annotation is identical to the Ensembl annotation.

简单翻译过来：

GENCODE的组成包括Havana组织的人工注释，以及Ensembl的程序自动注释，在Ensembl的genome浏览器中，使用的是GENCODE的注释文件。这两个是完全等价的！

那么GENCODE与Ensembl GTF一点区别也没有吗？肯定不是的！来看看官方文档怎么说：

>**What is the difference between GENCODE GTF and Ensembl GTF?**
>The gene annotation is the same in both files. The only exception is that the genes which are common to the human chromosome X and Y PAR regions can be found twice in the GENCODE GTF, while they are shown only for chromosome X in the Ensembl file.
In addition, the GENCODE GTF contains a number of attributes not present in the Ensembl GTF, including annotation remarks, APPRIS tags and other tags highlighting transcripts experimentally validated by the GENCODE project or 3-way-consensus pseudogenes (predicted by Havana, Yale and UCSC). Find here the complete list of tags.

简单翻译一下：

在X与Y的同源区域，有一些基因是在两条染色体上都有的，Ensembl只在X染色体注释了1次，GENCODE在X与Y染色体各注释了1次。

同时，<font color="#dd0000">GENCODE的GTF在第9列的可选列里面增加了很多新的tag信息来记录更多的注释内容。</font>这些在Ensembl的注释里面也是没有的。

