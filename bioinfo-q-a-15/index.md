# 【bioinfo】-Q&A-15

## 第15题 知道了BLAST，可是你知道BLAT吗？
Hello大家好！ 我们又见面了！又是新的一周，又是元气满满的一天！

今天我们讨论讨论低通量比对的最后一次问题，来为大家介绍一下BLAT。

可能看到BLAT这个名字，你可能会懵逼，不是BLAST吗？怎么少了个S？？？我们来为大家解释一下这个问题。

*BLAST = Basic Local Alignment **Search** Tool；*

*BLAT = BLAST-like alignment tool；*

从名字上，我们可以知道结论，BLAST是为了寻找1条SeqA的相似序列。一般输入的是1条FASTA序列，使用的参考序列是所有常见生物，包括微生物在内的非冗余数据库。特点是可以找到所有已知序列生物的相似序列。

*优点是全，缺点是有的时候输出冗余。*

比如，有时我只需要在某一种我想要的物种的基因组上进行快速的序列定位，结果BLAST却把所有的相似序列都找出来了。从时间上，还有从输出的冗余程度来说，这都不是最优解。所以BLAT工具应运而生！

BLAT的功能，简单来说就是我有1条序列SeqA，我想知道SeqA在**某一种基因组中的定位**（比如human genome）的工具。

在线的BLAT工具地址是[Human BLAT Search](http://genome.ucsc.edu/cgi-bin/hgBlat?command=start)；

或者可以通过打开UCSC genome browser --> tools --> BLAT的方式打开；

在UCSC genome browser中打开BLAT,点开BLAT以后的页面如下：

第1行是你要选择的基因组的参数，比如我们这里常用的就是人的参考基因组hg19版本。

下面的白框可以用来输入序列，用标准的FASTA格式就行。之后点submit就大功告成！

**我们今天的问题比较具体，也是我实际分析数据的时候遇到的1个问题。**

假设，我们有1个小RNA的测序结果，这些RNA的平均长度小于200bp；我们在比对到小RNA的序列库的候，发现有一个非常奇怪的现象。有2条miRNA的序列mapping到了非常多的reads数目，但是剩下的1800种miRNA总共才mapping到了10000条reads.
```
> 具体数目如下：
hsa-mir-7641-2	2000000
hsa-mir-7641-1	50000
```
通过miRBase检索，我们得到序列如下：
```
>hsa-mir-7641-2
GUUUGAUCUCGGAAGCUAAGCAGGGUCGGGCCUGGUUAGUACUUGGAUGGGAG
>>hsa-mir-7641-1
UCUCGUUUGAUCUCGGAAGCUAAGCAGGGUUGGGCCUGGUUAGUACUUGGAUGGGAAACUU
```
请尝试使用BLAT，Pairwise alignment等工具探索并解释原因。

双序列比对（Pairwise alignment）请使用下面的工具：

EMBOSS Water < Pairwise Sequence Alignment < EMBL-EBI

---

首先我们通过blat检索来看⼀一下这两条序列分别定位于染色体上的什什么位置，通过blat可得: hsa-mir-7641-2 和 hsa-mir-7641-1比对到了了5SRNA中间的⼀一段序列上

![](../../../../../Desktop/md/【bioinfo】-Q-A-15/1.png)

对5SRNA序列与两条目的序列分别做局部双序列比对:

hsa-mir-7641-1与5SRNA比对结果:

![](../../../../../Desktop/md/【bioinfo】-Q-A-15/2.png)

hsa-mir-7641-2与5SRNA⽐比对结果:

![](../../../../../Desktop/md/【bioinfo】-Q-A-15/3.png)

两条目标序列局部双序列比对结果:

![](../../../../../Desktop/md/【bioinfo】-Q-A-15/4.png)
```
 从上述比对结果可以看出:
 1.这2条目标序列与5SRNA中的一段序列高度重合;
 2.这2条目标序列重合的区域⾼度相似;
 结合前面的问题中提到本次实验属于小RNA的测序， 那么建库过程中核糖体RNA不可能完全去除，最终导致了之前的比对结果
 ```

