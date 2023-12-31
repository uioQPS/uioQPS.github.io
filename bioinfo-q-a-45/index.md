# 【bioinfo】-Q&A-45

## 高通量药物筛选对Rhabdoid tumors
> High-Throughput Drug Screening Identifies Pazopanib and Clofilium Tosylate as Promising Treatments for Malignant Rhabdoid Tumors

### 细胞系背景知识
重点关注Rhabdoid tumors (RTs) ，所以挑选了两种细胞系：
```
SMARCB1-positive cell lines，包括 I2A 和 (I2A + SMARCB1)
SMARCB1-negative cell lines (G401 and G402)
```
有意思的是 pazopanib 这个药物在这两个细胞系表现有统计学显著的区别。

还引入了另外5个人类Rhabdoid tumors (RTs) 细胞系：
```
KD (Versteege et al., 1998)
2004 (Versteege et al., 1998)
DEV (Giraudon et al., 1993)
Wa2 (Handgretinger et al., 1990)
LM (Versteege et al., 1998).
```
还有4个细胞系作为对照：
```
A673 (Ewing sarcoma)
DAOY (medulloblastoma)
A549 (SMARCA4-deficient lung carcinoma; obtained from the ATCC [CRL-1598, HTB-186, and CCL-185, respectively])
SK-N-BE(2)C (neuroblastoma) (Barnes et al., 1981).
```
### 药物筛选背景知识
药物筛选用384孔板，共纳入了1200种FDA批准的药物，以DAPI染色来判断药效。这1200种药物可以买到：Prestwick Chemical Library V2 (Prestwick Chemical), a collection of 1,200 off-patent drugs already approved by the FDA

重点挑选那些能显著抑制SMARCB1缺陷型细胞系生长的药物。

计算了药物的 **robust Z [RZ] score**来作为评价标准。(初筛)

### 总共挑选了10个药物
1) compounds showing a RZ score <−5 in at least one RT cell line (I2A or G401 cells) and showing no activity in I2A+ cells (RZ score >−2), which comprised 5 drugs (5-fluorouracil, luteolin, rimexolone, amethopterin [R,S], and methotrexate);

2) compounds with some weak activity in I2A+ cells (−5 < RZ score <−2) and showing a strong effect in at least one RT cell line (RZ score <−10) and a differential between I2A+ and I2A exceeding 2, leading to the selection of an additional 5 compounds (CfT, podophyllotoxin, ciclopiroxethanolamine, thioguanosine, and vatalanib).
详情如表格：Table 1. RZ Scores ，当然因为这些药物有希望，所以就又测了48小时的药物处理IC50 (μM)

只有6个药物满足在两个SMARCB1缺陷型细胞系都表现 (IC50) <10 μM ，其中四个都是anti-mitotic类别的，所以抛弃。

最后剩下的，值得期待的药物是：
```
one antiarrhythmic agent (clofilium tosylate [CfT])
broad TKi’s such as vatalanib, ponatinib, and pazopanib
其中CfT是potassium channel 的抑制剂
```
对于TKi’s药物，挑选了9个FDA批准的药物比较发现pazopanib最显著，后面的研究就替换掉了 vatalanib。

### 药物机制研究
因为TKi’s药物就是作用于 tyrosine kinase receptors (RTKs) ，所以作者用现成的商业的试剂盒： Proteome Profiler human phospho-RTK array 来探索 I2A, DEV, G401, and KD 这4种细胞系的 49个 RTKs的磷酸化情况。发现只有其中2个在药物pazopanib处理前后有区别：

- 首先是 platelet-derived growth factor receptors(PDGFRs) :
- 然后是 fibroblast growth factor receptor 2 (FGFR2)

还有Pazopanib和CfT的联合用药的体外细胞系实验。



### 表达芯片及RNA-Seq数据
在两个细胞系 (DEV and G401 ) 处理CfT 前后测转录组表达数据，共4*3=12个样本。

在I2A SMARCB1-def细胞系添加SMARCB1处理的时间序列，共(0,2,4,7,14day)*3=15个样本。

数据公布在GEO: GSE98277 : GSE102467
```
I2A microarray gene expression data
CfT-treated G401 and DEV RNA-seq data
```
仅仅是找了差异，然后根据阈值筛选到有统计学显著的差异基因做GO富集看看而已。

### PDX模型验证
制作了两个PDX模型：

- RT-001-HAM (liver tumor; Nicolle et al., 2016)
- IC-pPDX-6 (soft tissue; Curie Institute).

pazopanib showed actual growth delay on two extra-cranial RT PDXs as a single agent.

