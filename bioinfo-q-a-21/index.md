# 【bioinfo】-Q&A-21

## 第21题 SAM/BAM中的附加标记信息
Hello大家好！我们今天又见面了！

今天是第21题，我们接着之前的题目，继续学习与SAM/BAM有关的内容。今天要学习的内容是SAM/BAM文件的附加信息。

### 1. 基础导引部分

我们先给大家举个例子，这是一个human 的全基因组测序比对的SAM文件的11列以后的信息。第11列之前学习过了是reads的质量值，那么后面的若干标记比如MD:Z:145等等这些符号是什么意思呢？
![](../../../../../Desktop/md/【bioinfo】-Q-A-21/1.jpg)
图1 SAM文件的11列以后的信息截图

我把上面图中的部分行的信息放到这里，供大家查阅（11列以后的内容要一直向右拖拽）。
```
ST-E00126:128:HJFLHCCXX:2:1206:8105:9730	99	chr1	11670	1	145M	=	11898	315	AGGTGAAGCCCTGGAGATTCTTATTAGTGATTTGGGCTGGGGCCTGGCCATGTGTATTTTTTTAAATTTCCACTGATGATTTTGCTGCATGGCCGGTGTTGAGAATGACTGCGCAAATTTGCCGGATTTCCTTTGCTGTTCCTGC	KKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKFKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKFKKFKKKKKKKKKKKKKKKKKKKFFKKKKKKKKKKFKKKKKKKKKKKKKKKFAK	MD:Z:145	PG:Z:MarkDuplicates	XG:i:0	NM:i:0	XM:i:0	XN:i:0	XO:i:0	AS:i:0	XS:i:0	YS:i:0	YT:Z:CP
ST-E00126:128:HJFLHCCXX:2:2107:22820:18520	99	chr1	11682	1	145M	=	11920	325	GGAGATTCTTATTAGTGATTTCGGCTGGTGCCTGGCCATGTGTATTTTTTTAAATTTCCACTGATGATTTTGCTGCATGGCCGGTGTTGAGAATGACTGCGCAAATTTGCCGGATTTCCTTTGCTGTTCCTGCATGTAGTTTAAA	KKKKAAKKAFFKKKKKKFKFKKKFKKKKKKKKKKFKFKKKKKKKKKKKKKKFKFFKKKKKKFAAKAKKKKKKKKKKKKFFKKKFFFKKFKFFKKKKKKKKFFFFFKKKKKKK7<FFKKKKKKAFK<F<<7<AA,,7AA<7F7AA<	MD:Z:21G6G116	PG:Z:MarkDuplicates	XG:i:0	NM:i:2	XM:i:2	XN:i:0	XO:i:0	AS:i:-12	XS:i:-12	YS:i:-6	YT:Z:CP
ST-E00126:128:HJFLHCCXX:2:1210:9110:60026	163	chr1	11703	1	87M	=	11840	282	GGGCTGGGGCCTGGCCATGTGTATTTTTTTAAATTTCCACTGATGATTTTGCTGCATGGCCGGTGTTGAGAATGACTGTGCAAATTT	7FKAFKFKFFFKKF<FKKKFKKKFKK7F<KFFKFKKKKKKKFFFF,FKKKKKKKFFKKKKK(7<,AAK<F7AAFKKFKFKFF<A<7<	MD:Z:78C8	PG:Z:MarkDuplicates	XG:i:0	NM:i:1	XM:i:1	XN:i:0	XO:i:0	AS:i:-5	XS:i:-5	YS:i:-33	YT:Z:CP
ST-E00126:128:HJFLHCCXX:2:2101:7425:68324	99	chr1	11708	1	145M	=	11923	302	GGGGCCTGGCCATGTGTATTTTTTTAAATTTCCACTGATGATTTTGCTGCATGGCCGGTGTTGAGAATGACTGCGCAAATTTGCCGGATTTCCTTTGCTGTTCCTGCATGTAGTTTAAACGAGATTGCCAGCACCGGGTATCATT	KKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKK<FKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKAKKKKK<FKKKKFKKKKKKKKK7<AKFFAFKFF<KKKKKFKK<FK<7F,AFKFFA	MD:Z:145	PG:Z:MarkDuplicates	XG:i:0	NM:i:0	XM:i:0	XN:i:0	XO:i:0	AS:i:0	XS:i:0	YS:i:0	YT:Z:CP
ST-E00126:128:HJFLHCCXX:2:2210:15382:54752	163	chr1	11714	1	87M	=	11866	297	TGGCCATGTGTATTTTTTTAAATTTCCACTGATGATTTTGCTGCATGGCCGGTGTTGAGAATGACTGCGCAAATTTGCCGGATTTCC	KKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKFKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKK	MD:Z:87	PG:Z:MarkDuplicates	XG:i:0	NM:i:0	XM:i:0	XN:i:0	XO:i:0	AS:i:0	XS:i:0	YS:i:-5	YT:Z:CP
```
一般呢，我们都把11列以后的内容称为可选择区域（optional fields），这个区域所有的格式都必须是TAG:TYPE:VALUE的形式，比如MD:Z:145就是一个符合规范的可选区域的值。

根据SAM格式官方文档的信息，我们需要记住以下内容：
```
1. 所有的TAG都是2个字母，一般情况下都是大写字母。并且TAG在1行的比对结果中只能出现1次。
2. 所有的TYPE都是单字母，大小写敏感，它是用来定义后面VALUE的类型；
3. VALUE可长可短，但是需要和之前的TYPE相呼应。
```
关于TYPE不同字母对应的不同数据类型，把SAM的官方文档贴一下，共大家参考。其中，最常用的就是i（带符号的数字）；Z（可直接输出字符串，可以包含空格）；
![](../../../../../Desktop/md/【bioinfo】-Q-A-21/2.jpg)
图2 TYPE的字母与不同数据类型之间的对应关系

### 2.常用的TAG
那么常用的TAG都有哪些，都代表什么含义呢？要知道，不同的比对软件可能会在SAM文件的后面加上不同的TAG，所以我们在查询TAG含义的时候一定要从所用比对软件的官方文档中去查找。而SAM文件的header部分又包含了@PG字符段可以帮助我们还原比对软件的参数设置，因此我们拿到一个SAM文件就可以通过查阅文档的方式了解TAG的基本信息。
```
# 使用samtools可以查看sam文件的header部分
samtools view -H test.sam
```
比如，我们这里的@PG内容如下
```
@PG	ID:bowtie2-5DEB9F7A	PN:bowtie2	VN:2.2.5	CL:"/home/biotools/bowtie2-2.2.5/bowtie2-align-s --wrapper basic-0 -p 4 --phred33 -x /lustre/user/reference/hg19/hg19_combine -S ./tmp.data/fastq/genome-sequence.sam -1 ./tmp.data/fastq/genome-sequence_L3_1_trim5.fastq -2 ./tmp.data/fastq/genome-sequence_L3_2_trim5_92.fastq"
```
我们就知道这个test.sam文件使用了**bowtie2这个比对软件进行了比对**，因此我们在查询TAG信息的时候，我们就需要翻阅bowtie2的文档。

### 3. 提问环节
我们今天的问题很简单，请根据bowtie2的官方文档，解释下面的比对信息：
```
ST-E00126:128:HJFLHCCXX:2:2107:22820:18520	99	chr1	11682	1	145M	=	11920	325	GGAGATTCTTATTAGTGATTTCGGCTGGTGCCTGGCCATGTGTATTTTTTTAAATTTCCACTGATGATTTTGCTGCATGGCCGGTGTTGAGAATGACTGCGCAAATTTGCCGGATTTCCTTTGCTGTTCCTGCATGTAGTTTAAA	KKKKAAKKAFFKKKKKKFKFKKKFKKKKKKKKKKFKFKKKKKKKKKKKKKKFKFFKKKKKKFAAKAKKKKKKKKKKKKFFKKKFFFKKFKFFKKKKKKKKFFFFFKKKKKKK7<FFKKKKKKAFK<F<<7<AA,,7AA<7F7AA<	MD:Z:21G6G116	XG:i:0	NM:i:2	XM:i:2	XN:i:0	XO:i:0	AS:i:-12	XS:i:-12	YS:i:-6	YT:Z:CP
```
```
1. ST-E00126:128:HJFLHCCXX:2:2107:22820:18520
> 序列名称，比对片段的编号，通常包括测序平台的信息

2. 99
> Flag值

3. chr1
> 回帖到的染色体名称

4. 11682
> 比对到染色体上的具体位置(比对到正链最左边bp的位置点)

5. 1
> 比对的质量值，叫做MAPQ，MAPQ=-10 * log10{mapping出错的概率}

6. 145M
> CIGAR值，描述具体的⽐对情况

7. =
> pair reads中与该序列配对的read所mapping到的参考序列，如果没有mapping到同一条参考序列上，则⽤用“*”代替。

8. 11920
> pair reads中与该序列配对的read所mapping到的参考序列的具体位置

9. 325
> 通过分析pair reads mapping到同一条参考序列上位置的推断得到fragment的长度

10. GGAGA....TTAAA
> read序列信息

11. KKKKA....F7AA<
> read序列测序每一bp的质量量值

12. MD:Z:21G6G116
> MD:Z:表示在比对过程中有mismatch的情况，后面字符串表示mismatch的具体位置

13. XG:i:0
> XG:i有gap的存在，后面数字表示gap的总长度(read和reference上的都计算在内)

14. NM:i:2
> 编辑距离，为了将read map到reference上，对read进行单核苷酸编辑(替换、插⼊入以及删除)的最小长度

15. XM:i:2
> mismatche的具体数⽬

16. XN:i:0
> 序列覆盖区的参考基因组上不不确定的base数

17. XO:i:0
> gap的具体数目

18. AS:i:-12
> 比对分数，允许负值，局部比对最终可以大于0，但是全局比对中不会

19. XS:i:-12
> ⽐对过程中出现的⽐最终报告分数(AS:i:-12)⾼的⽐对值，同样允许负值，局部比对最终可以大于0，但是 全局比对中不会。当一条序列能够同时比对到多个位点，且出现连续局部相似度极⾼的情况下会出现这种情况。

20. YS:i:-6
 > 与该序列配对的pair read的⽐对分数

21. YT:Z:CP
> YT:Z:代表pair-read的比对情况，“UU”代表没有配对的read; "CP"代表序列为pair reads之一，pair align cordantly;"DP"表序列为pair reads之一，pair align discordantly;"UP"代表序列为pair reads之一，但是pair没有⽐对到参考基因组上。
```
参考资料：
[Bowtie 2-官方使用手册-SAM output部分](http://bowtie-bio.sourceforge.net/bowtie2/manual.shtml#sam-output)

