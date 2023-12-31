# 【bioinfo】-Q&A-28

## 第28题 如何使用samtools tview在命令行下对BAM文件进行可视化？
Hello 大家好！ 我们又见面了！

今天我们来讨论一下，怎么在命令行模式下对BAM文件进行快速地比对信息，也就算是命令行模式下的一种可视化了，虽然这种可视化没啥图形~

当然，如果你把BAM文件使用本地的工具也可以进行非常好的可是吧，比如使用IGV Tools。这个IGV Tools是一个我们最常用的可视化工具，我们在以后会进行详细地讨论，今天就只聊一聊命令行里这个的这个小工具—— samtools tview

### 1. 一些基本概念
其实，我们之前已经在第22题提醒过大家使用samtools tview这个工具了。它可以sort好的BAM文件进行可视化，并提供随机访问功能。

**测试文件**
```
# SAM测试文件的baidu盘下载地址
链接：https://pan.baidu.com/s/15gVVYPRu3VbF_uKbJUUGrA 密码：2drn
```
什么叫随机访问呢？

简单来说就是访问染色体任何1个位置的比对情况，速度差不多，而不受染色体先后顺序的影响。比如，我们比对时染色体是chr1，chr2，chr3…按顺序排列的，如果按顺序访问chr3的数据，就必须先读chr1，chr2的数据；而一点我们对BAM文件进行了sort，同时建立了BAM文件的index，那么就可以直接访问chr3的数据。

**BAM文件怎么排序及建立index？**

这个也很容易，使用samtools sort就好！我们在第22题的时候已经为大家简单介绍了。

大家需要注意，samtools sort是可以按reads的名称或reads的mapping结果进行排序的，我们这里一定要注意，需要按照mapping的结果进行排序，也就是mapping到染色体5’端的reads在前，靠3’端的reads在后。

在sort完以后，需要使用smtools index进行建立index，BAM index的名字为BAM文件名后加.bai
```python
# 对SAM文件转换成BAM文件
samtools view -hb ./test.sam > test.bam
# 对BAM文件按mapping结果进行排序
samtools sort -O BAM -T test_sort.bam.temp -o test_sort.bam  test.bam
# 对BAM文件建立index
samtools index test_sort.bam test_sort.bam.bai
```
### 2. 用一用samtools tview
```python
# samtools tview 用法
samtools tview test_sort.bam  ref.fa
# 注意，ref.fa文件mapping的时候使用的基因组的FASTA文件
```


使用上述命令，可以打开samtools tview的交互模式。需要注意，这里有一个坑，就是用conda安装的samtools不能使用交互模式，需要自己安装一下,按？！


里面的功能主要是方便我们快速定位，比如我们可以按“g”进行快速定位，如我们定位到chr1:12500位置。

此外，在samtools tview中，“.”代表和ref.fa内容完全一样；“，”表示比对到ref.fa的互补链；写了字母就代表mismatch。

### 3. 今天的问题
请你下载我们给出的SAM文件，并使用samtools 进行排序，然后使用samtools 查看chr1:13417附近的位置，你会发现在附近有1条reads出现了1个G->A的mutation。那么通过这1条reads是否能说明这个点是否真的位1个mutation？

请你在samtools tview的交互模式下，查看帮助文档，并熟悉相关操作。（使用conda安装的samtools可能不能正常使用samtools tview的交互功能）

注意：使用的参考基因组序列信息的下载地址
```
# hg19 1号染色体的FASTA下载地址
http://hgdownload.soe.ucsc.edu/goldenPath/hg19/chromosomes/chr1.fa.gz
# 下载后请解压缩
gzip -d chr1.fa.gz
```

