# 【bioinfo】-Q&A-40

## 深入了解snp-calling流程II
##### 操作：排序及标记重复

![](1.png)

1、排序（SortSam）


* 对sam文件进行排序并生成bam文件，将sam文件中同一染色体对应的条目按照坐标顺序从小到大进行排序
* GATK4的排序功能是通过picard SortSam工具实现的。虽然samtools sort工具也可以实现该功能，但是在GATK流程中还是推荐用picard实现，因为SortSam会在输出文件的头信息部分添加一个SO标签用于说明文件已经被成功排序，且这个标签是必须的，GATK需要检查这个标签以保证后续分析可以正常进行
* https://software.broadinstitute.org/gatk/documentation/tooldocs/current/picard_sam_SortSam.php
```
# 使用GATK命令
$ gatk SortSam -I mapping/T.chr17.sam -O preprocess/T.chr17.sort.bam -R database/chr17.fa -SO coordinate --CREATE_INDEX
# 使用picard命令
$ java -jar picard.jar SortSam \
      I=input.bam \
      O=sorted.bam \
      SORT_ORDER=coordinate
```

如何检查是否成功排序？
```
$ samtools view -H /path/to/my.bam
@HD     VN:1.0  GO:none SO:coordinate
@SQ     SN:1    LN:247249719
@SQ     SN:2    LN:242951149
@SQ     SN:3    LN:199501827
@SQ     SN:4    LN:191273063
@SQ     SN:5    LN:180857866
@SQ     SN:6    LN:170899992
@SQ     SN:7    LN:158821424
@SQ     SN:8    LN:146274826
@SQ     SN:9    LN:140273252
@SQ     SN:10   LN:135374737
@SQ     SN:11   LN:134452384
@SQ     SN:12   LN:132349534
@SQ     SN:13   LN:114142980
@SQ     SN:14   LN:106368585
@SQ     SN:15   LN:100338915
@SQ     SN:16   LN:88827254
@SQ     SN:17   LN:78774742
@SQ     SN:18   LN:76117153
@SQ     SN:19   LN:63811651
@SQ     SN:20   LN:62435964
@SQ     SN:21   LN:46944323
@SQ     SN:22   LN:49691432
@SQ     SN:X    LN:154913754
@SQ     SN:Y    LN:57772954
@SQ     SN:MT   LN:16571
@SQ     SN:NT_113887    LN:3994
```
若随后的比对记录中的contig那一列的顺序与头文件的顺序一致，且在头信息中包含SO:coordinate这个标签，则说明，该文件是排序过的

2、标记重复（Markduplicates）

* 标记文库中的重复
* https://software.broadinstitute.org/gatk/documentation/tooldocs/current/picard_sam_markduplicates_MarkDuplicates.php

```
gatk MarkDuplicates -I preprocess/T.chr17.sort.bam -O preprocess/T.chr17.markdup.bam -M preprocess/T.chr17.metrics --CREATE_INDEX
```


**质量值校正**

检测碱基质量分数中的系统错误，需要用到 GATK4 中的 BaseRecalibrator 工具

碱基质量分数重校准（Base quality score recalibration，BQSR)，就是利用机器学习的方式调整原始碱基的质量分数。它分为两个步骤:

>利用已有的snp数据库，建立相关性模型，产生重校准表( recalibration table)

>根据这个模型对原始碱基进行调整，只会调整非已知SNP区域。

注：如果不是人类基因组，并且也缺少相应的已知SNP数据库，可以通过严格SNP筛选过程（例如结合GATK和samtools）建立一个snp数据库。

注意：Base Recalibration是以read group为单位进行的，因此当一个BAM文件中包含了多个read group时，BaseRecalibrator会对每个read group分别建立校正模型

1、建立较正模型

质量值校正，这一步需要用到variants的known-sites，所以需要先准备好已知的snp，indel的VCF文件：
```
# 下载known-site的VCF文件，到Ensembl上下载
$ wget -c -P Ref/mouse/mm10/vcf ftp://ftp.ensembl.org/pub/release-93/variation/vcf/mus_musculus/mus_musculus.vcf.gz >download.log &
$ cd Ref/mouse/mm10/vcf && gunzip mus_musculus.vcf.gz && mv mus_musculus.vcf dbsnp_150.mm10.vcf
# 建好vcf文件的索引，需要用到GATK工具集中的IndexFeatureFile，该命令会在指定的vcf文件的相同路径下生成一个以".idx"为后缀的文件
$ gatk IndexFeatureFile -F dbsnp_150.mm10.vcf

# 建立较正模型
$ gatk BaseRecalibrator -R Ref/mouse/mm10/bwa/mm10.fa -I PharmacogenomicsDB/mouse/SAM/ERR118300.enriched.markdup.bam -O \
PharmacogenomicsDB/mouse/SAM/ERR118300.recal.table --known-sites Ref/mouse/mm10/vcf/dbsnp_150.mm10.vcf
```
2、质量值校准
```
# 质量校正
$ gatk ApplyBQSR -R Ref/mouse/mm10/bwa/mm10.fa -I PharmacogenomicsDB/mouse/SAM/ERR118300.enriched.markdup.bam -bqsr \
PharmacogenomicsDB/mouse/SAM/ERR118300.recal.table -O PharmacogenomicsDB/mouse/SAM/ERR118300.recal.bam
```

