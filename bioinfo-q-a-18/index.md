# 【bioinfo】-Q&A-18

## 第18题 高通量比对的质量MAPQ
Hello 大家好！ 我们又见面了！

今天我们接着昨天的内容，为大家介绍一下比对的质量MAPQ。

FASTQ格式的第4行记录的是每一个碱基的测序质量信息，也叫phred值。1个FASTQ记录的例子如下：
```
@HWI-ST1350:124:C1C2TACXX:3:1101:1223:2042
CTTTTCGAGTCAGACACATGACAGCCGGCAGCAACTGGAATGGCAGCAATT
+
BBCFFFFFGHHHHJJIJJIIJJJJIJJJGIJIIJJIJIGIIJJGIIIJIIG
```
我们在mapping的时候，会遇到一个问题，比如就用我们上面给大家展示的FASTQ序列举例。

如果这条序列（readA）最终可以比对到：

1号染色体的100000这个位置，但其中包含了1个mismatch（错配）；

或者是2号染色体的200000这个位置，但是有2个错配。

那readA到底是比对到第1个位置还是第2个位置呢？

这个时候就需要**1个度量值来帮我们做判断，选择1个最好的作为最终的比对结果**（当然研究一些比较特殊问题的时候需要把相似的比对结果都输出出来），这个度量值就是MAPQ。

### 那么MAPQ是什么意思呢？
根据SAM文件的官方定义：

>MAPQ: Mapping Quality. It equals -10 log10 Pr{mapping position is wrong}, rounded to the nearest integer. A value 255 indicates that the mapping quality is not available.

简单翻译一下：**MAPQ是mapping的质量值**，计算方法与FASTQ的质量值类似，
```
MAPQ=-10 * log10{mapping出错的概率}
```
当MAPQ=255的时候，代表MAPQ没有意义，就是一个占位符。

### 那么怎么计算MAPQ呢？
到了这里，可能又会有同学问了，虽然我们知道了MAPQ的含义，但是里面有一个mapping出错的概率，我应该怎么计算呢？这是一个非常容易问到的问题！

而我的回答是：

>根据mapping的情况，然后结合碱基的测序质量值进行评估。

**核心思想是，**

>低质量的碱基如果进行了mismatch（错配），那么很有可能是测序错误导致的，不应该罚太多分；   
>低质量的碱基如果与参考基因组完美match（匹配），那么也很有可能是测序错误导致的，不应该加太多分。

以我们下面的图1内容为例，第5列是MAPQ值，一般在后续分析的时候，我们都需要把MAPQ质量过低的reads去掉，**一般的cutoff是MAPQ≥10，严格一些的比如去寻找somatic mutation的时候需要MAPQ≥30.**

![](../../../../../Desktop/md/【bioinfo】-Q-A-18/1.jpg)
图1 标准的SAM文件截图

好了，说了这么多，我们今天的思考如下：

**1. 如果mapping的时候输入的是FASTA文件，那么MAPQ还有意义吗？为什么？**

没有意义。FASTA不包含测序质量信息，因此最后的MAPQ无法计算，也没有意义，常⽤用255代替。

**2. 不同的比对软件比如bwa与bowtie2，计算出来的MAPQ意义相同吗？为什么？**

BWA与Bowtie2的核心算法相同，但是比对策略和最终判断输出结果的评价体系不同。

MAPQ虽然代表的均是mapping的质量值，但是不同算法软件之间的MAPQ不能同时比较。

简单来说，我们不能认为BWA中MAPQ=42就要好于Bowtie2的MAPQ=40，反之亦然！

**3. 请写出samtools view 命令 获得MAPQ 大于等于20的sam文件，假设原始的sam文件名为raw.sam，过滤后的sam文件名为filter_MAPQ20.sam**
```
samtools view -S -q 20 ./raw.sam > ./filter_MAPQ20.sam
# -S input is sam file;
# -q INT minimum mapping quality ;
image-20200215010334720
```

