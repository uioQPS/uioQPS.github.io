# 【bioinfo】-Q&A-「1」

# FASTA 与 FASTQ格式详解
---
写在最前面的话：

为什么第2篇要写fastq和fasta这两种储存sequence的格式呢？主要是因为这两种数据储存格式是我们后续分析主要处理的两种数据格式。无论是后面的双序列比对（pairwise alignment）还是进化树构建（phylogenetic tree）亦或是高通量分析的回贴（mapping）数据的原始输入都是这两种格式。所以，为了后面更复杂的一些算法软件，我们就得先打好基础喽，Let’s go ！
***
## FASTA

概述一下，fasta格式是一种非常简单的储存序列的格式，可以储存核酸序列（DNA/RNA）也可以储存蛋白质的氨基酸序列（Amino Acid sequence，简称AA序列），主要分成2个部分。    
1. 是以“>”为开始的一行主要储存的是序列的描述信息；
2. 剩下的是序列部分，中间，前后都可以有空格。序列部分按照官方文档的说明应该是小于120就行，一般70到80左右。
3. 其实实际操作中，程序处理的时候都是自动去掉空格和换行符，把序列读成1行再处理，所以，我也干过把整条人类染色体都放到一行的233举动，这么算下来，一行可以有240*10E6这么长！~~~
举一个栗子！
### AA序列
```
>sp|P69905|HBA_HUMAN Hemoglobin subunit alpha OS=Homo sapiens GN=HBA1
MVLSPADKTNVKAAWGKVGAHAGEYGAEALERMFLSFPTTKTYFPHFDLSHGSAQVKGHG
KKVADALTNAVAHVDDMPNALSALSDLHAHKLRVDPVNFKLLSHCLLVTLAAHLPAEFTP
AVHASLDKFLASVSTVLTSKYR
```
这个例子是从UniRef数据库中下载的人类血红蛋白α亚基的序列。我们先看第1行：
```
>sp|P69905|HBA_HUMAN Hemoglobin subunit alpha OS=Homo sapiens GN=HBA1
```
这一行中，
* P69905是这个序列在UniRef中的编号
* HBA_HUMAN是这个序列的简称，Hemoglobin subunit alpha是全称
* OS=Homo sapiens是物种
* GN=HBA1是指基因的名字为HBA1.
* 后面的内容就是这个HBA1基因对应的蛋白的序列。其实根据官方文档，氨基酸都是用单字母表示，对应表如下：
```
A  alanine               P  proline       
B  aspartate/asparagine  Q  glutamine      
C  cystine               R  arginine      
D  aspartate             S  serine      
E  glutamate             T  threonine      
F  phenylalanine         U  selenocysteine      
G  glycine               V  valine        
H  histidine             W  tryptophan        
I  isoleucine            Y  tyrosine
K  lysine                Z  glutamate/glutamine
L  leucine               X  any
M  methionine            *  translation stop
N  asparagine            -  gap of indeterminate length
```
### 核酸序列


为了方便大家理解，我们继续使用人类血红蛋白a亚基对应的mRNA序列，这个序列是从NCBI RefSeq数据库中下载的。
```
>gi|13650073|gb|AF349571.1| Homo sapiens hemoglobin alpha-1 globin chain (HBA1) mRNA, complete cds
CCCACAGACTCAGAGAGAACCCACCATGGTGCTGTCTCCTGACGACAAGACCAACGTCAAGGCCGCCTGG
GGTAAGGTCGGCGCGCACGCTGGCGAGTATGGTGCGGAGGCCCTGGAGAGGATGTTCCTGTCCTTCCCCA
CCACCAAGACCTACTTCCCGCACTTCGACCTGAGCCACGGCTCTGCCCAGGTTAAGGGCCACGGCAAGAA
GGTGGCCGACGCGCTGACCAACGCCGTGGCGCACGTGGACGACATGCCCAACGCGCTGTCCGCCCTGAGC
GACCTGCACGCGCACAAGCTTCGGGTGGACCCGGTCAACTTCAAGCTCCTAAGCCACTGCCTGCTGGTGA
CCCTGGCCGCCCACCTCCCCGCCGAGTTCACCCCTGCGGTGCACGCCTCCCTGGACAAGTTCCTGGCTTC
TGTGAGCACCGTGCTGACCTCCAAATACCGTTAAGCTGGAGCCTCGGTGGCCATGCTTCTTGCCCCTTTG
G
```
对于第1行，
* 所有来自于NCBI的序列都有一个gi号，就是数据库中的流水号，具有唯一性。
* gb|AF349571.1是genebank编号的信息。
* 后面就是对序列的一个通俗的描述。这里使用的是mRNA，包含完整的CDS（Coding sequence）区域。

对于下面的序列，大家可能会有一个疑问，**为什么mRNA的序列还是用T来表示，而不是U？**其实这是为了保证数据的统一性，因为U只是在RNA中替换了原来的T，所以为了下游的方便分析处理，无论RNA序列还是DNA序列都是使用T而不是U。对于核苷酸序列的信息，我们一般使用下面的对应表。
```
A  adenosine          C  cytidine             G  guanine
T  thymidine          N  A/G/C/T (any)        U  uridine
K  G/T (keto)         S  G/C (strong)         Y  T/C (pyrimidine)
M  A/C (amino)        W  A/T (weak)           R  G/A (purine)        
B  G/T/C              D  G/A/T                H  A/C/T      
V  G/C/A              -  gap of indeterminate length
```
基本上FASTA就介绍到这里，这个格式主要是把序列储蓄到数据库中的一种格式，但是它不适合储存我们刚刚测到的测序数据。一个很重要的原因就是，**它没有序列的质量信息。**那一般带有测序质量信息的FASTQ格式就成了储存测序数据的常用格式啦！

## FASTQ

下面是一个Illumina平台测序的真实数据，其中包含了1条reads的信息。
```
@ST-E00126:128:HJFLHCCXX:2:1101:7405:1133
TTGCAAAAAATTTCTCTCATTCTGTAGGTTGCCTGTTCACTCTGATGATAGTTTGTTTTGG
+
FFKKKFKKFKF<KK<F,AFKKKKK7FFK77<FKK,<F7K,,7AF<FF7FKK7AA,7<FA,,
```
FASTQ格式储存的序列信息，每1条reads的信息，可以分成4行。
* 第1行主要储存序列测序时的坐标等信息
```
@ST-E00126:128:HJFLHCCXX:2:1101:7405:1133
@	开始的标记符号			
ST-E00126:128:HJFLHCCXX	测序仪唯一的设备名称
2	lane的编号				
1101	tail的坐标
7405	在tail中的X坐标
1133	在tail中的Y坐标
```
* 第2行就是测序得到的序列信息，一般用ATCGN来表示，**其中N表示荧光信号干扰无法判断到底是哪个碱基。**
* 第3行以“+”开始，可以储存一些附加信息，一般是空的。
* 第4行储存的是质量信息，与第2行的碱基序列是一一对应的，其中的**每一个符号对应的ASCII值成为phred值，可以简单理解为对应位置碱基的质量值，越大说明测序的质量越好。** 不同的版本对应的不同。


### 详细谈谈FASTQ质量值的计算方法

在测序仪进行测序的时候，会自动根据荧光信号的强弱给出一个参考的**测序错误概率**（error probility，P）根据定义来说，P值肯定是越小越好。我们怎么储存他们呢？直接储存成小数点？比如1%储存成0.01？这肯定是不高效的，因为1个碱基的信息，占用了至少4个字符。

所以科学家们的做法想了一个办法：

1.将P取log10之后再乘以-10，得到的结果为Q。
```
比如，P=1%，那么对应的Q=-10*log10（0.01）=20
```
2.把这个Q加上33或者64转成一个新的数值，称为Phred，最后把Phred对应的ASCII字符对应到这个碱基。
```
如Q=20，Phred = 20 + 33 = 53，对应的符号是”5”
```
这样就可以用1个符号与1个碱基一一对应，是不是很聪明？！



### 各版本不同Phred对应的ASCII值

* 在计算Q值和加上33/64的时候，不同测序仪，产生的数据不同，大概如下所示：

Solexa标准
![](../../../../../Desktop/md/【bioinfo】-Q-A-「1」/solexa.png)
Illumina标准
![](../../../../../Desktop/md/【bioinfo】-Q-A-「1」/illumina.png)




不同测序仪的不同Phred值对应的ASCII表
![](../../../../../Desktop/md/【bioinfo】-Q-A-「1」/phred.png)

