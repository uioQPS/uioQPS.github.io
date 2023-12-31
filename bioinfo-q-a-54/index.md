# 【bioinfo】-Q&A-54

## 探索FGFR3-TACC3基因融合机制
#### FGFR3-TACC3基因融合机制探究
A metabolic function of FGFR3-TACC3gene fusions in cancer, 美国哥伦比亚大学医学中心（CUMC）的研究人员发现两个相邻基因的融合能够导致线粒体过度运转和增加细胞疯狂生长所需的燃料数量，从而导致癌症产生。他们也发现在人癌细胞和一种脑癌类型的小鼠模型中，靶向这个新鉴定出的癌症通路的药物能够阻止肿瘤生长。

全文的重点应该是实验探索及验证FGFR3-TACC3基因融合机制。

#### FGFR3-TACC3基因融合事件
作者在以前的GBM研究（2012年发表在Science期刊上的一项研究）中就发现了约有3%的FGFR3-TACC3基因融合现象，而且同时期很多其它癌症也发现了同样的融合事件。虽然FGFR3-TACC3基因融合表现出**原癌基因的特性，会影响FGFR抑制剂的效果**，但是它的下游调控机制研究的并不清楚。

>题外话：有趣的是我分析了一个台湾人的OSCC cohort的转录组数据，也发现了这个FGFR3-TACC3基因融合现象。

论文共同通信作者、CUMC癌症遗传学研究所神经病学教授、病理学与细胞生物学教授Antonio Iavarone博士说，“**这可能是人类癌症中唯一最为常见的基因融合。**我们想要确定FGFR3-TACC3融合如何诱导和维持癌症，这样我们就可能鉴定出药物治疗的新靶标。”

#### 线粒体活性
长期以来，人们已在癌症中观察到线粒体变化，但是仅在近期才发现线粒体活性和细胞代谢与某些癌症存在关联。然而，**基因突变改变线粒体活性和促进肿瘤生长的机制是未知的。**

在当前的这项研究中，这些研究人员比较了携带着FGFR3-TACC3和未携带着FGFR3-TACC3的癌细胞中的上千个基因的活性。他们发现这种融合极大地增加线粒体的数量和增加它们的活性。鉴于癌细胞需要大量的能量才能快速地分裂和生长，当线粒体的活性增加时，这些癌细胞就能够茁壮生长。

通过采用多种实验技术，这些研究人员确定这种基因融合启动了一系列增强线粒体活性的事件。

- 首先，FGFR3-TACC3激活一种被称作PIN4的蛋白。一旦遭受激活，PIN4迁移到过氧化物酶体中。
- 作为一种细胞器，过氧化物酶体将脂肪降解为增加线粒体活性的物质。受到激活的PIN4触发产生的过氧化物酶体数量增加了4～5倍，从而释放大量的氧化剂。
- 最后，这些氧化剂诱导PGC1α产生。作为线粒体代谢的一种关键的调控物，PGC1α增加线粒体的活性和能量产生。

说明了mitochondrial respiration可以作为有FGFR3-TACC3基因融合发生的那部分癌症患者的治疗靶点。



#### 表达芯片数据分析
作者通过实验手段构造了含有FGFR3-TACC3融合基因的astrocyte细胞系，看它们经过了FGFR tyrosine kinase (TK, PD173074) or vehicle处理后的基因表达区别，还有一个比较特殊的组是 kinase-dead F3–T3 (F3–T3(K508M)) ，这个虽然FGFR3-TACC3也融合，但是有一个突变位点，导致其表达的产物是不具备激酶活性的。

分组如下：
```
F3T3_1  FGFR3-TACC3 treated with vehicle
F3T3_2  FGFR3-TACC3 treated with vehicle
F3T3_3  FGFR3-TACC3 treated with vehicle
F3T3_4  FGFR3-TACC3 treated with vehicle
F3T3_5  FGFR3-TACC3 treated with vehicle
F3T3_KD_1   Empty vector treated with vehicle
F3T3_KD_2   Empty vector treated with vehicle
F3T3_KD_3   Empty vector treated with vehicle
F3T3_PD_1   FGFR3-TACC3 treated with the FGFR inhibitor PD173074 for 12h
F3T3_PD_2   FGFR3-TACC3 treated with the FGFR inhibitor PD173074 for 12h
F3T3_PD_3   FGFR3-TACC3 treated with the FGFR inhibitor PD173074 for 12h
F3T3_PD_4   FGFR3-TACC3 treated with the FGFR inhibitor PD173074 for 12h
F3T3_PD_5   FGFR3-TACC3 treated with the FGFR inhibitor PD173074 for 12h
VECTOR_1    FGFR3-TACC3-K508M treated with vehicle
VECTOR_2    FGFR3-TACC3-K508M treated with vehicle
VECTOR_3    FGFR3-TACC3-K508M treated with vehicle
表达芯片数据可以下载，E-MTAB-6037 ， 用的是 Illumina HumanHT-12 V4.0 expression beadchip
```
既然有4个组，那么就可以按照自己的想法来两两组合的比较，找差异，看变化，注释，讲生物学故事。

差异热图如下：






化合物PD173074是辉瑞公司研发的一个化合物.现在,已知它是FGFR1的强抑制剂.

比较了FGFR3-TACC3基因融合细胞系被药物处理前后的全局基因表达变化，发现它们GO富集分析主要集中在： mitotic activity, oxidative phosphorylation and mitochondrial biogenesis 。








#### TCGA公共数据分析
从TCGA里面下载了GBM的表达数据，进行了Mann–Whitney–Wilcoxon (MWW) test statistics (ee-MWW) 分析，因为534例GBM患者里面只有9例样本是有FGFR3-TACC3基因融合现象的，然后作者也纳入了仅有的10例正常脑部组织，对差异基因做热图并且GO注释如下：






其中，上图中有FGFR3-TACC3基因融合的9个样本标记为红色。

同时也分析了TCGA数据库的其它癌症的FGFR3-TACC3基因融合样本与其它样本的区别，同样是ee-MWW分析方法。都发现了F3–T3 fusion gene correlated with mitochondrial activities 然后又用了 regularized gradient boosting machine 算法来检测FGFR3-TACC3基因融合造成的基因表达变化模式相关的转录因子，发现跟 PPARGC1A and ESRRG 类似。 而且有FGFR3-TACC3基因融合现象的样本里面的 PPARGC1A and ESRRG 表达量的确增高了。

补充材料里面还利用了TCGA数据，绘制其它图。




#### MWW富集分析方法
Mann-Whitney-Wilcoxon秩和检验

Wilcoxon-Matt-Whitney test (or Wilcoxon rank sum test, orMann-Whitney U-test) 用于比较两个并不满足正态分布群组的均值比较：这是一个非参数检验（non-parametrical test）。其与应用于独立样本的t-test相当。

代码公开：
A collection of the R procedures to perform ee-MWW is available at http://github.com/miccec/yaGST. The RGBM package is available from CRAN at https://cran.r-project.org/web/packages/RGBM/index.html.



#### 衍生阅读-FGFR-TACC融合基因
《Science》在2009年首次报道胶质瘤中存在FGFR-TACC融合基因，该融合基因过度表达的GBM具有高度侵袭性。因此，FGFR-TACC可能确立一种新的GBM亚型，或成为潜在的治疗靶点。法国巴黎Sorbonne大学总医院Anna Luisa Di Stefano等在2019年1月《Clin Cancer Res》上发表对FGFR3-TACC3融合基因在IDH野生型胶质瘤中的表达特点及作用的研究结果。

该项研究收集795例胶质瘤患者标本，包括584例GBM，85例II-III级IDH野生型胶质瘤，126例II-III级IDH突变型胶质瘤，开展FGFR3-TACC3融合基因检测。85例II-III级IDH野生型胶质瘤中，3例存在FGFR3-TACC3融合基因，而在II-III级IDH突变的胶质瘤中均未检测出该融合基因。在584例GBM中，17例存在FGFR3-TACC3融合基因

