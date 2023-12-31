# 【bioinfo】-Q&A-12

## 第12题 trim与cutadapt 先用哪个？
Hello大家好！我们又见面了！

上一次我们说到了cutadapt软件的使用问题，其中我们着重强调了1个参数-m，不知道大家还有没有印象。今天我们问题就是要使用-m参数和另一个工具联合的一个妙用。不过在这之前，我们还得先介绍1个工具箱叫fastx_toolkit.

fastx_toolkit是一个系列内容的软件包，其中主要的内容是对比对前的fastq文件做质控。比如切掉一些不要的内容（**fastx_trimmer**），比如FASTQ与FASTA格式的转换（**fastq_to_fasta**），比如分单链测序的index（**fastx_barcode_splitter**）等等。我们今天主要是给大家说一下fastx_trimmer的用处。

fastx_trimmer主要是切掉一些fastq中你不想要的序列，比如有些序列5’端有若干bp的质量不好的或者碱基不稳定的部分；或者是5’端有一些用来去重复（duplicate）的random barcode（如图1所示）；还可能是3’端一些质量不好的碱基。

![](../../../../../Desktop/md/【bioinfo】-Q-A-12/1.jpg)
图1 前面10bp序列含有random barcode因此在比对之前需去掉。

这里我再给大家1张图，就是之前我们展示过的Human普通的RNA-Seq测序的adapter分布图（图2）。
![](../../../../../Desktop/md/【bioinfo】-Q-A-12/2.jpg)

图2-1 Human普通的RNA-Seq测序的adapter分布图

**在实际数据分析与处理的过程中，会有下面几个要求：**

- fastq文件中的adapter肯定是需要去掉的；

- 一些**头部的random barcode也是需要去掉的**；

- 在进行一些特殊的分析的时候，还需要保证所有的输入序列长度完全一致，不能长不能短，必须整整齐齐在一起（比如RNA-Seq的可变剪切分析经常有这个要求）。

**那么我们今天的问题就是——**

### 假设你有一个RNA-Seq测序文件需要进行可变剪切分析，你需要达到的要求是：
```
1. 处理过后的fastq文件中不包含adapter序列；
2. 处理过后的fastq文件中的开头10bp是random barcode也需要去；
3. 最后得到的序列长度完全一致（不满足上述要求的扔掉）；
假设我们的输入文件是：input.fastq
假设我们的操作系统是：Linux Ubuntu
其中的adapter序列是：AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGTAGATCTCGGTGGTCGCCGTATCATT
```
请参考cutadapt与fastx_trimmer这两个软件的使用说明，设计处理路线图。如果有可能，请给出相应的参数。
```
为了了达到最终⽬目的，处理理过程主要分为3步:
第1步:
    使⽤用fastqc获得序列列3'端adapter以及5'端random barcode的信息;
第2步:
    使⽤用cutadapt去掉3'端的adapter，务必注意需要使⽤用-m参数，只保留留⼀一定⻓长度以上的序列列;
第3步:
    使⽤用fastx_trimmer去除5'端不不想要random barcode，同时截取相同⻓长度的序列列，保证最后得到的序列列⻓长度完全⼀一致。

参考代码及参数如下:

# step 1, FastQC
fastqc -q -t 3 -o ./FastQC_result ./input.fastq

# step 2, cut adapter
cutadapt -25 -m 125 \
-a AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGTAGATCTCGGTGGTCGCCGTATCATT \
-o input_cutadapt.fq.gz input.fastq &

# step 3, trim
zcat input_cutadapt.fq.gz | fastx_trimmer \
-f 11 -l 125 -z -o ./input_cutadapt_trim11_125.fq.gz
# 经过上述3个步骤，可以获得⻓长度统⼀一为115bp的序列!
```

