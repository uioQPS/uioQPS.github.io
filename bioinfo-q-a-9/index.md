# 【bioinfo】-Q&A-「9」

## 第9题 读懂FastQC报告中的duplicate问题
Hello大家好！

我们又见面了！本周我们预计会把前10个问题提出来，结束我们的测序原理与FastQC部分。

今天我们来详细聊聊duplicate问题。

duplicate的产生主要是因为Illumina建库的过程中，一般会需要使用PCR来帮助扩增插入序列的浓度。

在扩增的过程中，**如果PCR扩增轮数过大，就会出现duplicate的问题，即产生一模一样的若干条序列**。

FastQC中“Sequence Duplication Levels”图是用来刻画duplicate情况的。

img图1 duplicate结果图

那么我们今天的问题如下：

**1. 图1中的横坐标是什么意思，纵坐标是什么意思？**

横坐标代表序列重复水平，纵坐标代表重复水平序列占所有序列的百分比

**2. 图1中的红线和蓝线分别代表什么意思？**

红线：代表去duplicate之后序列理论重复性分布（服从possion distribution或者binomial distribution）情况

蓝线：代表全部的序列重复性分布情况

**3. 图1中的duplicate是全部序列的duplicate的情况吗？还是随机筛选了一部分？为什么要这样做？**

是选择的每一个文件里面**前100, 000条序列**作为样本进行的计算，因为样本本身很大，前100, 000已经能够代表样本的重复性。

**4. 如果让你写程序，判断1个fastq文件中duplicate的比例，你的大概思路是什么？**
```
# Python风格的伪代码
# 第1步，对序列进行排序
sort the FASTQ file by the sequence, and names as sorted_file;
# 第2步，对排序的序列统计是否为duplicate
total_num = 1
duplication_num = 0
reads_1 = sorted_file.readline()
for reads_2 in sorted_file:
    if reads_1 == reads_2
        duplication_num += 1
    else:
        reads_1 = reads_2
    total_num += 1
print(total_num)
print(duplication_num)
```
**额外的思考题：
既然谈到了duplicate的问题，那就存在remove duplicate的问题，什么情况下应该去duplicate，什么情况下不去除？ 最好的去除办法是什么？（仅需要思考一下，以后我们会有专题讨论这个问题）**

DNA-seq中，序列如果是随机打断，需要考虑deduplication；酶切样本一般不需要考虑这个问题；

RNA-seq中，一般不考虑deduplication（有paper专门讨论过这个问题）；

但鞋包测序需要建库过程中添加random barcode，且必须考虑deduplication

[FastQC官方说明文档-Duplicate Sequences](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/3%20Analysis%20Modules/8%20Duplicate%20Sequences.html)

