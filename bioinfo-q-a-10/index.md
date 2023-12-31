# 【bioinfo】-Q&A-「10」

## 第10题 读懂FastQC报告之adapter与kmer
Hello大家好！

我们又见面了！今天是我们的FastQC中最后1次提问啦！今天，我们要聊得是adapter与kmer的问题。

我们在[第5题 测序建库的adapter](http://47.98.132.251/2020/02/16/【bioinfo】-Q-A-「5」) 的时候讨论过adapter的问题，我们知道adapter的最主要的作用是为了能够与flowcell连接，方便进行桥式PCR。那么我们的fastq文件中到底含不含adapter呢？FastQC报告就能告诉我们。

同时呢，我们今天还会讨论kmer的问题，相关的报告FastQC也会输出出来。

Part I adapter部分

img

图 1-1 1个正常的adapter报告

img

图 1-2 1个RNA-Seq的adapter报告

Part II kmer部分

img

图 2-1 正常的RNA-Seq建库kmer统计

img

图 2-2 加入random barcode的RNA-Seq建库kmer统计

img

图 2-3 kmer的统计显著性分析

### 关于adapter的问题：
**1. Illumina的通用adapter序列是什么？图1-1与图1-2中的各种不同颜色的图例是什么意思？**
```
Illumina Paired End Adapters(cannot be used for multiplexing)
Top adapter
    5' ACACTCTTTCCCTACACGACGCTCTTCCGATC*T 3'
Bottom adapter
    5' P-GATCGGAAGAGCGGTTCAGCAGGAATGCCGAG 3'

**来源**
http://bioinformatics.cvr.ac.uk/blog/illumina-adapter-and-primer-sequences/
不同颜色的图例代表不同的测序通用的adapter；
如果在当时fastqc分析的时候-a选项没有内容，则默认使用图例中的通用adapter序列进行统计。
```

**2. 图1-1与图1-2中的横坐标与纵坐标分别是什么意思？**

横坐标代表reads中的位置，纵坐标代表adapter序列含量的百分比

**3. 图1-1与图1-2中最显著的差异是什么？如果两者都是RNA-Seq的数据，哪个可以继续下游分析，哪个不能够进行下游分析？为什么？**

图1-1，结果表明少量的序列3’ 端检测到了少部分的adapter序列，且测序时使用的是红色图例所代表的的adapter；

图1-2，结果表明测序时使用的是红色图例代表的adapter；除了前20bp，reads后半段序列有很高的比例都是adapter，建库可能存在问题。

### 关于kmer的问题：
如果某k个bp的短序列在reads中大量出现，其频率高于统计期望的话，fastqc将其记为over-represented k-mer。默认的k = 5，可以用**-k –kmers**选项来调节，范围是2-10。出现频率总体上3倍于期望或是在某位置上5倍于期望的k-mer被认为是over-represented。fastqc除了列出所有**over-represented k-mers，还会把前6个的per base distribution画出来**。

当有出现频率总体上3倍于期望或是在某位置上5倍于期望的k-mer时，报”黄色!”；当有出现频率在某位置上10倍于期望的k-mer时报”红色×”。本图所显示的结果来自于表格中前六个序列。

**1. kmer就是一定长度的序列，比如AATTCCGG就可以叫做8-mer。那么图2-1余图2-2中的横坐标什么意思？纵坐标什么意思？**

横坐标代表短序列的长度，纵坐标代表某长度的短序列在所有reads中所出现的频率百分比

**2. 图2-1与图2-2中哪个kmer问题比较严重？为什么？**

图2-2 的问题更严重，因为该图中kmer的出现位置集中且数量较多，可能是加入了random barcode，出现了duplication问题

**3. 图2-2中是在reads的5’端加入了约10bp左右的随机序列，结合 第9题 读懂FastQC报告中的duplicate问题 这样做的目的是什么？**

一般在进行RNA-seq测序时，是不会进行remove duplication；

但是一些比较特殊的建库流程，比如单细胞RNA-seq测序时，PCR扩增轮数较多，可能出现大量的duplication；

对于这些建库方法，通常需要添加random barcode，然后需要根据random barcode进行remove duplication

**思考题：
图2-3是FastQC生成的kmer是否显著的统计报告。其中的每一列是什么意思？这个统计显著性检验计算的p-value是使用什么方法计算的？**
```
第一列：kmer内容
第二列：kmer在序列中某一位置出现的观测数量
第三列：二项分布统计检验P-value
第四列：kmer在某一位置的观察值与理论值的比值
第五列：观察值与理论值比值最高值出现的位置
```

