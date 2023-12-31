# 【bioinfo】-Q&A-24

## 第24题 GFF，GTF到底是什么？
Hello大家好！我们又见面了！

在第23问的时候，我们开始学习了转录组相关的生物信息学。在转录组分析的过程中，往往需要基因注释文件，通常是GFF或者是GTF文件，那么这两个文件的内容是什么？有什么特点呢？这是我们今天要探索的问题。

### 1. 我们为什么需要基因注释文件？
通过对外显子（exon）的可变剪切，同1个基因可以形成多种蛋白（https://en.wikipedia.org/wiki/Alternative_splicing）

我们的gene在基因组上的结构不是连续的，而是exon-intron-exon（exon=外显子，intron=内含子）分隔开的。

基因要表达，首先会先发生转录过程，转录出包含intron的pre-mRNA序列，然后再进行可变剪切，加5’帽子，3’ PolyA尾巴等一系列复杂的加工过程才会形成成熟的mRNA。

在进行转录组序列比对，尤其是mRNA序列比对的时候，经常需要处理**跨越两个exon之间的reads**，所以在进行序列比对的时候往往需要对基因组有一个注释，**告诉比对软件哪个位置是gene的exon，哪个地方是gene的intron，这个就是我们所说的基因注释信息。**所以，<u>它里面核心内容就是一大堆gene在基因组上的坐标，以及这个gene本身的一些属性。</u>

### 2. GTF与GFF都是基因注释文件
GTF = General Transfer Format

GFF = General Feature Format

GFF有若干个版本，简单来说，GTF是GFF文件的其中一个版本，我们一般认为GTF文件就是GFF 2.0版本的内容。一个标准的GTF/GFF2.0文件需要包括9列内容，一个简单的示意图如下：

![](../../../../../Desktop/md/【bioinfo】-Q-A-24/1.jpg)

图2 1个标准的GTF格式文件，文件不包括前面的行号

这9列分别是什么呢？ 我试着为大家翻译ensembl网站的说明文件（GFF/GTF File Format）。

```
# 所有的列必须用TAB分隔，总共有9列内容，第9列是补充列；
# 补充列的内容可以为空，但是前面8列必须有内容，如果想表达空的概念，则需要用"."；
# 第1列 seqname
染色体的名称，需要与genome FASTA文件中的染色体名对应，别一个用"chr1"一个用"Chr1"；
# 第2列 source
注释来自哪里，比如图2表示来自NCBI RefSeq数据库；
# 第3列 feature
此行的注释类型，一般有exon，CDS，stop_codon, start_codon等等；
# 第4，5列 start，end
此行注释的起始和终止位置，标准的GTF/GFF都是以1为染色体的起点（1-based system）;
注意！无论这个gene是正链还是负链，start的坐标都小于end坐标；
# 第6列 score
一般存放打分值，比如拼装的可信度之类。下载的官方注释文件一般为0.0
# 第7列 strand
正链基因标记为 "+", 负链基因标记为 "-";
# 第8列 frame
只可能是0,1,2这3个值,表示与CDS中codon的相对位置；
0表示，这个region的第1bp就是正好是codon 三连密码子的第1个碱基；
1表示，这个region的第2bp就是正好是codon 三连密码子的第1个碱基；
2表示，这个region的第3bp就是正好是codon 三连密码子的第1个碱基；
# 第9列 attribute
一般会记录 gene_id 与transcript_id;
这一列是可选列，可以增加很多内容。在程序处理过程中，相同的attribute会合并在一起处理。
比如，所有gene_id=SGIP1的行都会先汇总在一起，表示1个基因。
```
### 3. 提出问题
既然是这样，我们今天就思考2个小问题，1个比较偏理论，1个是比较具体。

**1. 你认为GTF/GFF的文件格式设计合理吗？为什么？**

并不是非常合理，这种格式虽然包括了注释所需要的全部信息，但是同一个基因不同的elements并不在一行，在mapping的时候需要用循环一行一行去判断该element是否是同属于一个基因，比较耗费十佳你和内存，如果不选择GTF文件，合适选择下图所示的“all files from selected table”文件，这种文件的注释信息的组合方式与GTF不同，是以一个基因为一行，包括了**这个基因中的各个elements**，这样的助释放阀使得比对过程更加便捷
![](../../../../../Desktop/md/【bioinfo】-Q-A-24/2.png)
```
all files from selected table ⽂文件内容示例例:
#bin name chrom strand txStart txEnd cdsStart cdsEnd exonCount ex onStarts exonEnds score name2 cdsStartStat cdsEndStat exonFrames
1251 NM_004261.4chr1 - 87328127 87380048 87329156 87379794 5
  87328127,87333735,87346344,87368963,87379710,   87329288,87333785,87346408,87369
131,87380048,   0   SELENOF cmpl    cmpl    0,1,0,0,0,
```
**2. 如果告知，transcript_id为NM001308203.1*，*gene_id为SGIP1, 在转录本上的坐标为101，那么对应基因组的坐标是多少？请写出答案与简要程序思路。注释信息如下：**

```
chr1	hg19_ncbiRefSeq	exon	66999252	66999355	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	start_codon	67000042	67000044	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67000042	67000051	0.000000	+	0	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	66999929	67000051	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67091530	67091593	0.000000	+	2	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67091530	67091593	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67098753	67098777	0.000000	+	1	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67098753	67098777	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67105460	67105516	0.000000	+	0	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67105460	67105516	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67108493	67108547	0.000000	+	0	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67108493	67108547	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67109227	67109402	0.000000	+	2	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67109227	67109402	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67136678	67136702	0.000000	+	0	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67136678	67136702	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67137627	67137678	0.000000	+	2	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67137627	67137678	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67138964	67139049	0.000000	+	1	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67138964	67139049	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67142687	67142779	0.000000	+	2	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67142687	67142779	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67145361	67145435	0.000000	+	2	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67145361	67145435	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67154831	67154958	0.000000	+	2	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67154831	67154958	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67155873	67155999	0.000000	+	0	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67155873	67155999	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67160122	67160187	0.000000	+	2	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67160122	67160187	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67184977	67185088	0.000000	+	2	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67184977	67185088	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67194947	67195102	0.000000	+	1	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67194947	67195102	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67199431	67199563	0.000000	+	1	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67199431	67199563	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67205018	67205220	0.000000	+	0	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67205018	67205220	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67206341	67206405	0.000000	+	1	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67206341	67206405	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67206955	67207119	0.000000	+	2	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67206955	67207119	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	CDS	67208756	67208775	0.000000	+	2	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	stop_codon	67208776	67208778	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
chr1	hg19_ncbiRefSeq	exon	67208756	67216822	0.000000	+	.	gene_id "SGIP1"; transcript_id "NM_001308203.1";
```
判断的思路是根据注释信息计算chr1上的exon的分布与长度，转录本上的坐标值代表了该片段之前的exon长度，那么以该题为例,chr1上该基因第一个exon的长度是6699355-6699252+1=104，转录本坐标是101，说明该片段就是map到这个exon上的，坐标是6699355+101-1=6699455

