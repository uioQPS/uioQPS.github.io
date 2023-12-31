# 【bioinfo】-Q&A-30

## 第30题 IGVTools使用的一些小技巧

闲话少说，Let’s go！
```
目录
1. IGV Tools支持的文件类型与绘图方式；
2. 提供RNA-Seq的mapping测试数据；
3. IGV Tools设置Reads的排序方式；
4. IGV Tools画Sashimi plot；
5. 使用IGV Tools浏览公共数据；
6. 视频教程部分
```
### 1. IGV Tools支持的文件类型与绘图方式
我们之前已经介绍过IGV Tools可以对多种数据进行可视化，那么IGV Tools支持的数据类型有哪些呢？根据官方手册的介绍IGVTools主要支持下列数据类型：
![](1.jpg)
表1-1 IGV Tools支持的数据格式与数据内容 （http://software.broadinstitute.org/software/igv/DefaultDisplay）

通过表1-1我们知道，通过IGV Tools我们可以载入FASTA文件作为参考序列；可以载入BAM文件读取比对信息；可以载入BED（一般是各种peak的注释信号）还可以载入GFF/GTF的基因注释文件；当然还有一些特殊格式比如mutation的数据等等。对于这些数据，IGV Tools会用哪些方式来进行可视化呢？
![](2.jpg)
表1-2 数据类型可视化标准 （http://software.broadinstitute.org/software/igv/DefaultDisplay）

根据表1-2所示，对于不同数据类型，IGV Tools有自己的可视化方式。大家其实不必完全记住这些内容，有一个大概的印象即可，关键是要会用这个工具。

### 2. 提供RNA-Seq的mapping测试数据
为了方便大家学习，我为大家准备了一点RNA-Seq的mapping好的BAM文件与BAM-index文件。我们接下来的分析也都以这套数据为例进行操作。
```
链接：https://pan.baidu.com/s/1nORmpD9DU6yK4zzmWan_lw 密码：orbz
```
### 3. IGV Tools设置Reads的排序方式
我们向IGV Tools提供的BAM文件必须是按参考基因组坐标排序过的，且需要提供BAM文件的index 文件（bai 文件），但是对于IGV Tools来说，当我们zoom in一段区域以后，比如缩放到TP53这个基因区域，我们是可以对这个区域里面的reads进行各种方式排序与分组的。

图3-1显示的是 TP53基因，reads按比对到参考基因组位置排序的可视化图；
![](3.jpg)
图3-1 TP53基因， reads按比对到参考基因组位置排序（默认）
![](4.jpg)
图3-2 TP53基因，reads按照 start location进行排序

图3-2是不是会比图3-1整齐很多呢？这是因为使用了start location进行排序（视频会有介绍），一般在绘制publish的图时，都需要进行对齐操作好让结果比较美观。常用的还有若干种排序方式：
![](5.jpg)
图3-3 常见的排序方式

### 4. sashimi plot
sashimi plot是对可变剪切的一种可视化方式，类似图4-1，它可以标记处有多少跨越intron的reads，方便我们观察可变剪切，比如skip exon的reads数目的变化。
![](6.jpg)
图4-1 sashimi plot 例子（http://software.broadinstitute.org/software/igv/Sashimi）

在IGV Tools里面可以非常容易生成类似的图，只需要轻轻一点！~

说句题外话，大家知道为什么这种图叫sashimi plot吗？

sashimi其实就是我们经常说的刺身，大家不觉得这种一个小峰，一个小junction很像刺身吗？

### 5. 使用IGV Tools浏览公共数据
IGV Tools里可以方便我们下载各种公共数据，比如ENCODE数据，比如TCGA的数据，完全傻瓜操作，就是轻轻一点公共数据立马下载下来，并且自动可视化，是不是挺方便的？

H3K4me3的是最常用的定位active promoter region的一个信号。简单来说，就是有H3K4me3 peak出现的地方一般认为是转录比较活跃的区域，尤其是promoter区域。

