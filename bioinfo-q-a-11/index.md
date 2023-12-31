# 【bioinfo】-Q&A-「11」

## 第11题 使用cutadapt去除adapter
Hello大家好！我们又见面了！

通过前面的生物信息学10个基础问题，我相信大家对测序的基本原理，FASTA与FASTQ格式以及FastQC的质控报告都有了一个清楚的认识。那么接下来，我们就要进一步学习，学习如何把原始的FASTQ测序结果一步一步的准备成可以用来比对（mapping）的质控过后的FASTQ。

我们知道，测序结果中可能会有若干条序列存在adapter的信息，而adapter的信息一般是不在基因组上存在的。所以，在比对之前如果不把adapter去干净，我相信你会得到1个非常非常低的mapping rate。

img

图1 RNA-Seq建库的结果，如果不去adapter接下来根本比对不上！

通常情况下，**我们都是使用cutadapt这个软件进行adapter（接头）序列的去除。cutadapt这个软件不但支持单端序列，还支持双端序列的切除，同时还支持gz格式的自动压缩与解压缩。**1个常用的切除命令类似：
```
# 在linux 命令行模式下
cutadapt -a ADAPTER_FWD -A ADAPTER_REV -o out.1.fastq -p out.2.fastq reads.1.fastq reads.2.fastq
# -a是第1个文件的adapter序列
# -A是第2个文件的adapter序列
# -o是第1个输出文件
# -p是第2个输出文件
# reads.1.fastq 是第1个输入文件，也就是双端测序中的read-1
# reads.2.fastq 是第2个输入文件，也就是双端测序中的read-2
```
那么我们今天需要思考的问题，与切除adapter的具体内容有关。

**1. cutadapt中-a/-A 参数与-g/-G参数分别代表什么意思？Illumina测序过程中，一般不会用到哪个参数？**
```
-a 后⾯面跟⼀一段核苷酸序列列，代表单端测序中3'端需要去掉的adapter;
-A 代表双端测序中，read2的3'端需要去掉的adapter; 注意:如果是双端测序数据，-a针对read1起作⽤用;

-g 后⾯面也分别是⼀一段核苷酸序列列，代表单端测序中5'端adapter序列列;
-G 的⽤用法可以类⽐比-a/-A，是针对reads2的5'端的adapter;

通常情况下，Illumina测序过程中5'端不不会测出adapter序列列，所以-G/-g⼀一般不不会被⽤用到。
```
**2. cutadapt可以过滤一些非常短的reads，请解释其中-m 参数是什么意思？为什么要过滤一些非常短的reads？**
```
-m 后⾯面跟着是⼀一个数字;
意思是当每条read切完adapter后如果序列列⻓长度低于这个数字就会被舍弃， 因为如果序列列⻓长度过短，则这条read不不宜被⽤用于后续的mapping。
```
**3. 请说明用哪个参数来设置相关的cutoff，并简要说明cutadapt对read质量判断的策略与方法。**

img

图2 一般3’端的序列质量不够好，即使去掉adapter以后还是需要把低质量的序列再去除1次，从而保证后续的mapping质量。
```
使⽤用-q命令可以再在cut adapter时将测序质量量值差的序列列去掉，设定⼀一个质量量值，低于该分数的bp将被 切掉，具体命令是:
    > cutadapt -q 10 -o output.fastq input.fastq

需要注意的是,-q后⾯面的数字表示质量量值的低阈值，如果FASTQ⽂文件质量量值采⽤用pherd33标准，则不不⽤用备注;
但如果FASTQ⽂文件采⽤用phred64标准，则需要在代码中添加 “--quality-base=64”。
--------------------
cutadapt 去除3'端低质量量序列列的核⼼心算法
--------------------
phred = -10 × log10(Error Probability)

假设我们有13bp的序列列，其序列列和phred值分别为:
    A,T,G,C,C,G,T,A,C,C,G,G,T,T,A
    42, 42, 41, 41, 40, 26, 27, 8, 7, 11, 4, 2, 3

假设我们的-q参数选择10，⾸首先先对所有序列列的质量量值减去这个-q后⾯面的参数，结果:
    32, 32, 31, 31, 30, 16, 17, -2, -3, 1, -6, -8, -7
随后从3'⽅方向向5'⽅方向进⾏行行累加，结果如下:
    164, 132, 100, 69, 38, 8, -8, -25, -23, -20, -21, -15, -7
则从3'到5'遇到累加结果的第1位⼤大于0的位置即为保留留的第1位;
所以最终保留留cut的结果为:
    A,T,G,C,C,G,T,A--cut here--C,C,G,G,T,T,A

这样做的好处是可以得到⽐比较稳定的⾼高测序质量量值，防⽌因为3'端某个质量量值突然提⾼高⽽而终⽌去除。
```
```
fastx_trimmer -i test_data_2.fastq -o test_data_2_trim.fastq -f 1 -l 85
```
[cutadapt 1.16官方文档-Trim低质量区域的算法说明](https://cutadapt.readthedocs.io/en/stable/guide.html?highlight=-q#quality-trimming-algorithm)

