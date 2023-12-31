# 【bioinfo】-Q&A-39

## 深入了解snp-calling流程I
## GATK4流程


### 准备配套数据
要明确你的参考基因组版本了！！！ b36/b37/hg18/hg19/hg38，记住b37和hg19并不是完全一样的，有些微区别哦！！！

**下载hg19**

这个下载地址非常多，常用的就是NCBI，ensembl和UCSC了，但是这里推荐用这个脚本下载（下载源为UCSC）：
```
# 一个个地下载hg19的染色体
for i in $(seq 1 22) X Y M;
do echo $i;
wget http://hgdownload.cse.ucsc.edu/goldenPath/hg19/chromosomes/chr${i}.fa.gz;
done

gunzip *.gz

# 用cat按照染色体的顺序拼接起来，因为GATK后面的一些步骤对染色体顺序要求非常变态，如果下载整个hg19，很难保证染色体顺序是1-22，X,Y,M
for i in $(seq 1 22) X Y M;
do cat chr${i}.fa >> hg19.fasta;
done

rm -fr chr*.fasta
```
BWA: Map to Reference

建立参考序列索引
```
$ bwa index -a bwtsw ref.fa
参数-a用于指定建立索引的算法：
```
bwtsw 适用于>10M   
is 适用于参考序列<2G (默认-a is)
可以不指定-a参数，bwa index会根据基因组大小来自动选择合适的索引方法

**序列比对**
```
$ bwa mem ref.fa sample_1.fq sample_2.fq -R '@RG\tID:sample\tLB:sample\tSM:sample\tPL:ILLUMINA' \
	2>sample_map.log | samtools sort -@ 20 -O bam -o sample.sorted.bam 1>sample_sort.log 2>&1
-R 选项为必须选项，用于定义头文件中的SAM/BAM文件中的read group和sample信息
```

> The file must have a proper bam header with read groups. Each read group must contain the platform (PL) and sample (SM) tags.
> For the platform value, we currently support 454, LS454, Illumina, Solid, ABI_Solid, and CG (all case-insensitive)
>
> The GATK requires several read group fields to be present in input files and will fail with errors if this requirement is not satisfied
>
> ID：输入reads集的ID号； LB： reads集的文库名； SM：样本名称； PL：测序平台
>
查看SAM/BAM文件中的read group和sample信息：
```
$ samtools view -H /path/to/my.bam | grep '^@RG'
@RG ID:0    PL:solid    PU:Solid0044_20080829_1_Pilot1_Ceph_12414_B_lib_1_2Kb_MP_Pilot1_Ceph_12414_B_lib_1_2Kb_MP   LB:Lib1 PI:2750 DT:2008-08-28T20:00:00-0400 SM:NA12414  CN:bcm
@RG ID:1    PL:solid    PU:0083_BCM_20080719_1_Pilot1_Ceph_12414_B_lib_1_2Kb_MP_Pilot1_Ceph_12414_B_lib_1_2Kb_MP    LB:Lib1 PI:2750 DT:2008-07-18T20:00:00-0400 SM:NA12414  CN:bcm
@RG ID:2    PL:LS454    PU:R_2008_10_02_06_06_12_FLX01080312_retry  LB:HL#01_NA11881    PI:0    SM:NA11881  CN:454MSC
@RG ID:3    PL:LS454    PU:R_2008_10_02_06_07_08_rig19_retry    LB:HL#01_NA11881    PI:0    SM:NA11881  CN:454MSC
@RG ID:4    PL:LS454    PU:R_2008_10_02_17_50_32_FLX03080339_retry  LB:HL#01_NA11881    PI:0    SM:NA11881  CN:454MSC
...
read group信息不仅会加在头信息部分，也会在比对结果的每条记录里添加一个 RG:Z:* 标签
```
```
$ samtools view /path/to/my.bam | grep '^@RG'
EAS139_44:2:61:681:18781    35  1   1   0   51M =   9   59  TAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAA B<>;==?=?<==?=?=>>?>><=<?=?8<=?>?<:=?>?<==?=>:;<?:= RG:Z:4  MF:i:18 Aq:i:0  NM:i:0  UQ:i:0  H0:i:85 H1:i:31
EAS139_44:7:84:1300:7601    35  1   1   0   51M =   12  62  TAACCCTAAGCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAA G<>;==?=?&=>?=?<==?>?<>>?=?<==?>?<==?>?1==@>?;<=><; RG:Z:3  MF:i:18 Aq:i:0  NM:i:1  UQ:i:5  H0:i:0  H1:i:85
EAS139_44:8:59:118:13881    35  1   1   0   51M =   2   52  TAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAA @<>;<=?=?==>?>?<==?=><=>?-?;=>?:><==?7?;<>?5?<<=>:; RG:Z:1  MF:i:18 Aq:i:0  NM:i:0  UQ:i:0  H0:i:85 H1:i:31
EAS139_46:3:75:1326:2391    35  1   1   0   51M =   12  62  TAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAA @<>==>?>@???B>A>?>A?A>??A?@>?@A?@;??A>@7>?>>@:>=@;@ RG:Z:0  MF:i:18 Aq:i:0  NM:i:0  UQ:i:0  H0:i:85 H1:i:31
...
若原始SAM/BAM文件没有read group和sample信息，可以通过AddOrReplaceReadGroups添加这部分信息

$ java -jar picard.jar AddOrReplaceReadGroups \
      I=input.bam \
      O=output.bam \
      RGID=4 \
      RGLB=lib1 \
      RGPL=illumina \
      RGPU=unit1 \
      RGSM=20
```
### 前期处理
在进行本部分的操作之前先要做好以下两部的准备工作

1. 创建GATK索引。用Samtools为参考序列创建一个索引，这是为方便GATK能够快速地获取fasta上的任何序列做准备
```
$ samtools faidx database/chr17.fa	# 该命令会在chr17.fa所在目录下创建一个chr17.fai索引文件
```
2. 生成.dict文件
```
$ gatk CreateSequenceDictionary -R database/chr17.fa -O database/chr17.dict
GATK4的调用语法：
gatk [--java-options "-Xmx4G"] ToolName [GATK args]
```
#### 去除PCR重复

##### duplicates的产生原因


PCR duplicates（PCR重复）     
PCR扩增时，同一个DNA片段会产生多个相同的拷贝，第4步测序的时候，这些来源于同！一！个！拷贝的DNA片段会结合到Fellowcell的不同位置上，生成完全相同的测序cluster，然后被测序出来，这些相同的序列就是duplicate

Cluster duplicates     
生成测序cluster的时候，某一个cluster中的DNA序列可能搭到旁边的另一个cluster的生成位点上，又再重新长成一个相同的cluster，这也是序列duplicate的另一个来源，这个现象在Illumina HiSeq4000之后的Flowcell中会有这类Cluster duplicates

Optical duplicates（光学重复）     
某些cluster在测序的时候，捕获的荧光亮点由于光波的衍射，导致形状出现重影（如同近视散光一样），导致它可能会被当成两个荧光点来处理。这也会被读出为两条完全相同的reads

Sister duplicates     
它是文库分子的两条互补链同时都与Flowcell上的引物结合分别形成了各自的cluster被测序，最后产生的这对reads是完全反向互补的。比对到参考基因组时，也分别在正负链的相同位置上，在有些分析中也会被认为是一种duplicates。

##### 用泊松分布解释 NGS 测序数据的 duplication 问题

我曾经有这样的疑惑，为什么文库构建过程中的 PCR 将每个文库分子都扩增了上千倍，以 PCR 10个循环为例 210= 1024 ，但是实际测序数据中 duplication 率并不高（低于20%）

有一篇文章从统计概率的角度详细探讨了一下 duplication 率的影响因素

PCR 的过程中不同长度的文库分子被扩增的效率不同（GC 太高或 AT 含量太高都会影响扩增效率），PCR 更倾向于扩增短片段的文库分子，这里先不考虑文库片段扩增效率的差异，把问题简化一下，假设所有文库分子扩增效率都相同。PCR duplicate 的主要来源是同一个文库分子的不同拷贝都在 flowcell 上生成了可以被测序的 cluster ，导致同一个分子的序列被测序仪读取多次。那么为何在每个分子都有上千个拷贝的情况下，实际却很少出现同一分子的多个拷贝被测序的情况呢？主要原因就是文库中 unique 分子的数量比被 flowcell 上引物捕获的分子数量多很多，直白点说就是 flowcell 上用于捕获文库分子的引物数量太少了，两者不在同一个数量级，导致很少出现同一个文库分子的多个拷贝被 flowcell 上引物捕获生成 cluster。

假设文库中所有分子与引物的结合都是随机的，简化一下就相当于，一个箱子中有 n 种颜色的球（文库中的 n 种 unique 分子），每种颜色有 1000 个（PCR 扩增的，随 cycle 数变化），从这个箱子中随机拿出来 k 个球（最终测序得到 k 条 reads），其中出现相同颜色的球就是 duplicate，那么 duplication 率就可以根据有多少种颜色的球被取出 0,1,2,3…… 次的概率计算，可以近似用泊松分布模型来描述。

以人全基因组重测序 30X 为例，PE150 需要约 3x108条 reads ，文库中 unique 分子数其实可以通过上机文库的浓度和体积（外加 PCR 循环数）计算出来，这里用近似值 3.5x1010 个 unique 分子。每个 unique 分子期望被测序的次数是 3x108/3.5x1010 = 0.0085 ，每个 unique 分子被测 0,1,2,3… 次的概率如下图：

> x <- seq(0,10,1)
> xnames <- as.character(x)
> xlab <- "一个文库分子的所有拷贝被测序的次数"
> ylab <- "概率"
> barplot(dpois(x,lambda = 0.0085),
+ names.arg = xnames,
+ xlab = xlab,
+ ylab = ylab)


由于 unique 分子数量太多，被测 0 次的概率远高于 1 和 2 次，我们去除 0 次的看一下：



unique 分子被测序 1 次的概率远大于 2次及以上，即便一个 unique 分子被测序 2 次，我们去除 duplicate 时候还会保留其中一条 reads。

如果降低文库中 unique 分子数量到 4.5x109个，PCR 循环数增加以便浓度达到跟上面模拟的情况相同，测序 reads 数还是 3x108 条，每个 unique 分子预期被测序的次数是 3x108/4.5x109 = 0.067 。



unique 分子数量减少，被测序 2次的概率增大，duplication 率显然也会增高。

到这里已经可以很明白的看出 duplication 率主要与文库中 unique 分子数量有关，所以建库过程中最大化 unique 分子数是降低 duplication 率的关键。文库中 unique 分子数越多，说明建库起始量越高，需要 PCR 的循环数越少，而文库中 unique 分子数越少，说明建库起始量越低，需要 PCR 的循环数越多，因此提高建库起始量是关键。

##### PCR bias的影响    
DNA在打断的那一步会发生一些损失， 主要表现是会引发一些碱基发生颠换变换（嘌呤-变嘧啶或者嘧啶变嘌呤） ， 带来假的变异。 PCR过程会扩大这个信号， 导致最后的检测结果中混入了假的结果；

PCR反应过程中也会带来新的碱基错误。 发生在前几轮的PCR扩增发生的错误会在后续的PCR过程中扩大， 同样带来假的变异；

对于真实的变异， PCR反应可能会对包含某一个碱基的DNA模版扩增更加剧烈（这个现象称为PCR Bias） 。 因此， 如果反应体系是对含有reference allele的模板扩增偏向强烈， 那么变异碱基的信息会变小， 从而会导致假阴。

##### 探究samtools和picard去除read duplicates的方法
1、samtools

samtools 去除 duplicates 使用 rmdup
```
$ samtools rmdup [-sS] <input.srt.bam> <out.bam>
```
只需要开始-s的标签， 就可以对单端测序进行去除PCR重复。其实对单端测序去除PCR重复很简单的，因为比对flag情况只有0,4,16，只需要它们比对到染色体的起始终止坐标一致即可，flag很容易一致。

但是对于双端测序就有点复杂了~

In the paired-end mode, this command ONLY works with FR orientation and requires ISIZE is correctly set. It does not work for unpaired reads (e.g. two ends mapped to different chromosomes or orphan reads).

很明显可以看出，去除PCR重复不仅仅需要它们比对到染色体的起始终止坐标一致，尤其是flag，在双端测序里面一大堆的flag情况，所以我们的94741坐标的5个reads，一个都没有去除！

这样的话，双端测序数据，用samtools rmdup效果就很差，所以很多人建议用picard工具的MarkDuplicates 功能

2、picard

picard对于单端或者双端测序数据并没有区分参数，可以用同一个命令！

对应单端测序，picard的处理结果与samtools rmdup没有差别，不过这个java软件的缺点就是奇慢无比

