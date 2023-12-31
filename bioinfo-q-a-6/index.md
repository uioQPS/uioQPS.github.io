# 【bioinfo】-Q&A-「6」

## 读懂FastQC报告 Part I
Hello 大家好！

通过前面的5个问题，我相信大家对Illumina测序，测序的储存文件格式，一些简单的建库原理已经有了一个初步的认识。那么接下来，我们就要用我们学到的知识去解决一些问题啦。

在实际操作和处理过程中，我们拿到的Illumina测序数据应该是.fastq.gz格式，其中gz表示的是使用gzip进行压缩，fastq表示使用fastq格式进行存储。获得数据的第一步，通常就是使用FastQC软件进行质控。

FastQC会对每一个输入的fastq.gz文件生成1个html网页和一个zip的压缩包。压缩包里是网页中包含的图片信息，因此我们只需要看网页里面整理好的内容就好。

今天的问题围绕着FastQC的质控图来展开，请看下面2张图。

img

图1 - 1个Illumina测序结果， reads1 的 per-base quality boxplot

img

图2 - 1个Illumina测序结果， reads2 的 per-base quality boxplot

问题如下：

---
1. 图中的横坐标表示什么意思？

横轴是测序序列的第1个碱基到第150个碱基

2. 图中的纵坐标表示什么意思？

总左边表示每一bp所对应的测序质量值

前面讲过将该碱基判断错误概率值P取log10之后再乘以-10

得到的结果再加上Phred值对应ASCII表所得到的的值就是该碱基测序的质量值

Q = -10*log10(error P)

即20表示1%的错误率，30表示0.1%的错误率

---

3. 图中的蓝色线是什么意思？

蓝色的细线是各个位置的质量值的平均值的连线

4. 图中的box 下面的bar ， 上面的bar，箱体的下沿，箱体的上沿，箱体内部的横线分别代表什么意思？

每1个boxplot，都是该位置的所有序列的测序质量的一个统计

上面的bar是90%分位数

下面的bar是10%分位数

箱子的中间的横线是50%分位数

箱体上缘是75%分位数

箱体下缘是25%分位数

分位数解释：

​ 如果一组数的25%分位数是a，意味着a超过了这组数中25%数字的大小

---

5. 图1与图2最主要的区别在哪里？结合我们之前的问题，为什么会出现这种情况？

相比于reads 1的测序结果，reads 2的测序质量均匀性差，准确率低

主要原因：
reads 2 的测序是在 reads 1 150 bp 测序完成后
forward strands再通过1次桥式PCR合成reverse strands
这之后再进行荧光测序
测序质量差的主要原因是因为长时间测序结束后，合成酶的活性降低，导致合成时加不上一些碱基，最终同步性变差，主要属于phasing错误.

---
