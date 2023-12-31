# 【bioinfo】-Q&A-55

## pan-cancer

#### 泛癌研究之拷贝数变异
发表于 September 2013 Pan-cancer patterns of somatic copy number alteration

#### 对拷贝数的分类整理
We identified a total of 202,244 SCNAs, a median of 39 per cancer sample, comprising 6 categories:

- focal SCNAs that were shorter than the chromosome arm (median of 11 amplifications and 12 deletions per sample);
- arm-level SCNAs that were chromosome-arm length or longer (median of 3 amplifications and 5 deletions per sample);
- copy-neutral loss-of-heterozygosity (LOH) events in which one allele was deleted and the other was amplified coextensively (median of 1 per sample);
- whole-genome duplications (WGDs; in 37% of cancers).

By amplifications and deletions, we refer to copy number gains and losses, respectively, of any length and amplitude.

#### 使用 ABSOLUTE分析肿瘤纯度
#### 统计学显著的拷贝数区域
把经过肿瘤纯度矫正后的拷贝数情况输入到GISTIC软件，参数包括:-
```
a noise threshold of 0.3,
a broad length cutoff of 0.5 chromosome arms,
a confidence level of 95% and a copy-ratio cap of 1.5.

```
#### 泛癌研究之使用MutSigCV看肿瘤突变异质性
Mutational heterogeneity in cancer and the search for new cancer-associated genes

作者团队主要是开发了一款算法软件：MutSigCV

We apply MutSigCV to exome sequences from 3,083 tumour–normal pairs and discover extraordinary variation in mutation frequency and spectrum within cancer types, which sheds light on mutational processes and disease aetiology, and in mutation frequency across the genome, which is strongly correlated with DNA replication timing and also with transcriptional activity.



#### 泛癌研究之肿瘤驱动突变
 Comprehensive identification of mutational cancer driver genes across 12 tumor types

作者提供数据分析得到 291个高置信度的肿瘤驱动突变基因 acting on 3,205 tumors from 12 different cancer types.

其实主要是测试4种软件：
```
MuSiC
OncodriveFM
OncodriveCLUST[
ActiveDriver
```
#### 泛癌研究之肿瘤突变全景
 Mutational landscape and significance across 12 major cancer types

