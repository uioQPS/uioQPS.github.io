# 【bioinfo】-Q&A-58

## 多组学探索不同器官的小细胞癌症起源
文章是：Reprogramming normal human epithelial tissues to a common, lethal neuroendocrine cancer lineage 小细胞癌症里面最出名的应该是小细胞肺癌(SCLC)了，恶性度高,预后差,治疗上进展也比较少。小细胞肺癌大约占所有肺癌的15%,每年全国有十几万新发的小细胞肺癌患者,其中绝大多数患者诊断的时候就已经是晚期。所以针对它的研究，就有点类似于乳腺癌里面的TNBC一样。

有趣的是，大部分公众号解读这篇文章的标题都侧重于指出：Science:如何将健康细胞癌化?只需五个基因

一些名词：
```
small cell neuroendocrine carcinoma (SCNC)
reprogrammed prostate and lung SCNCs
small cell prostate cancer (SCPC)
small cell lung cancer (SCLC)
euroendocrine prostate cancer (NEPC)
poorly differentiated prostate adenocarcinoma (PrAd)
large cell prostate carcinoma
```

很多不同器官的癌症会发展成具有很强侵略性的小细胞癌，也称为小细胞神经内分泌癌（SCNC）来抵制治疗。不同器官的癌症类型是否通过相同途径转变成SCNC细胞还不清楚。肺癌、前列腺癌、膀胱癌和其他组织的小细胞癌在名称上是相似的，但是临床医生通常把它们当作不同的实体来治疗，然而，在过去的几年里，研究者们逐渐开始认识到癌症有相似之处。


- 关键发现：前列腺和肺细胞在健康时有着非常不同的基因表达模式，但是当它们转化成小细胞癌时，几乎具有相同的模式。研究表明，不同类型的小细胞瘤的演变相似，即使它们来自不同的器官。



#### 3种ngs组学测序数据
通过将带有五个基因的人类前列腺细胞(统称为PARCB)移植到小鼠体内，探索了癌症类型之间的潜在相似之处。当这些细胞在小鼠体内生长时，它们显示出人类小细胞神经内分泌癌的独特特征，基因如下；






不同基因的组合的分组就可以通过NGS手段来探索他们的差异。

GSE118204 包含另外3个GSE数据集，分别是RNA-seq，Whole Exome-seq，ATAC-seq
```
gseinfo GSE118204Reprogramming normal human epithelial tissues to a common, lethal neuroendocrine cancer lineage (ATAC-seq)GSE118205Reprogramming normal human epithelial tissues to a common, lethal neuroendocrine cancer lineage (Whole Exome-seq)GSE118206Reprogramming normal human epithelial tissues to a common, lethal neuroendocrine cancer lineage (RNA-seq)GSE118207PARCB Project: Reprogramming normal human epithelial tissues to a common, lethal neuroendocrine cancer lineage
```
但是作者的分析结果主要集中于 RNA-seq和ATAC-seq，看了看应该是Whole Exome-seq数据是没有配对正常组织测序结果来辅助寻找somatic的mutation信息。

#### RNA-seq数据分析结果
转录组的分析流程是：






通过上面的分析流程得到表达量矩阵后，作者最重要的分析点是强调这些不同组别的细胞系的相关性，如下：








携带着PARCB的SCNC细胞（PARCB-SCNC细胞）与来自人体的小细胞前列腺癌（small cell prostate cancer, SCPC）细胞存非常相似,，如下图，SCLC和PARCB聚集在一起。





这些分析都需要比较强的背景知识，还有对公共数据的把控能力。

#### ATAC-seq数据分析结果
同样也是先看看其数据分析流程：






因为数据都是公布出来的，所以可以比较轻松的下载下来，走一波我的教程：https://www.jianshu.com/p/5bce43a537fd 看看能不能得到同样的分析结果图表。

主要也是根据信号值来进行PCA等看看不同组的相关性如何，如下图：
![](1.jpg)





文章关于ATAC-seq数据独特的分析点不多，就：








#### 联合分析
对于RNA-seq和ATAC-seq数据的联合分析，作者做得并不多，流程描述如下：

