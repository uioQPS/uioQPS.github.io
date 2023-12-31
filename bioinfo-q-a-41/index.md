# 【bioinfo】-Q&A-41

## 探究为何一个TNBC对gefitinib过于敏感

- A Targetable EGFR-Dependent Tumor-Initiating Program in Breast Cancer

### 背景知识

1. epidermalgrowth factor receptor (EGFR)
2. EGFR inhibition by gefitinib
3. triple-negative breast cancer (TNBC)
4. patient-derived xenografts (PDXs)
5. Deep single-cell RNAsequencing of 3,500 cells

### 实验设计
作者制作了一批TNBCs (15/18)的PDX模型，然后用这些模型来测试其对 EGFR inhibitor gefitinib 敏感情况。前人报道该药物在TNBC病人里面有效率是38.7%，与他们的实验想符合(6/18), 但是其中有一个人的反映比较特殊，就是 GCRC1735， 一个70岁的老奶奶，该药物治疗效果出奇的好。实验表明是正中靶点，降低了pEGFR，同时下调了pERK通路，激活了凋亡通路。基因检测表明该老奶奶有一个 pathogenic BRCA1 mutation (p.C1225Sfs) 和a somatic TP53 alteration (p.R249T) ，而EGFR基因上面既没有突变也没有拷贝数变异，EGFR 这个通路相关的基因也没有太大的异常，没办法解释该病人为何会对gefitinib如此敏感，值得探究。

### 单细胞转录组测序
因为bulk测序无法解决问题

To understand functional properties associated with heterogeneous EGFR expression in an unbiased manner, single cell RNA-seq was performed on freshly dissociated cells from the PDX (3,483 cells, with an average of 40,564 unique molecular identifiers (UMIs) and 5,146 genes detected per cell)

数据都在SRA数据库里面，如下：

https://trace.ncbi.nlm.nih.gov/Traces/study/?acc=SRP100090

https://trace.ncbi.nlm.nih.gov/Traces/study/?acc=SRP110989

其中单细胞测序是对一个70岁的老奶奶做的，所有的数据都在SRP110989里面，共3500个细胞，分析结果如下：

### Cells partitioned into six subpopulations
```
a mesenchymal/stem (M/S) cluster ，表现为 EGFR 和mesenchymal相关基因高表达
the nuclear/mitochondrial cluster，表现为 nuclear 和 mitochondrial基因定位
a proliferative cluster，表现为细胞周期相关基因高表达
the antigen presentation cluster，表现为抗原呈递相关基因高表达。
the basal cluster ，包括 (KRT6A/B, KRT17, KRT14, and KRT5) 等markers
A transitional cluster ， 差异基因太少
```
#### subpopulation-specific markers
用 diffuse t-SNE 进行分组后，找到 marker 基因

#### pathway analysis
对找到的marker基因进行注释

分析了5个公共数据：
上面得到的各个cluster 的 gene list需要跟公共数据进行对比注释
```
BRCA1 and familial breast cancers (GSE49481)(Larsen et al., 2014)
pre/post-chemotherapy (GSE32072)(Gonzalez-Angulo et al., 2012)
FACS normal mammary cell (GSE16997)(Lim et al., 2009)
(GSE37223)(Kannan et al., 2013)
FACS normal mammary cells single-cell qPCR (GSE70554)(Lawson et al., 2015).
主要是看差异表达，还有用 GOBO 算法看 signature scores ​
```
#### 使用 Monocle 进行 trajectory 推断
这个是算法问题，可以先略过，直接看结论。

#### 乳腺癌表达分型
拿到了乳腺癌患者的表达矩阵，就可以根据ssp2003 , ssp2006 and pam50来对其进行分类

Breast cancers were divided into luminal A, luminal B, HER2+ and basal-like subtypes using the ssp2003 , ssp2006 and pam50 classi ers, via the ‘genefu’ R package (http://www.bioconductor.org/packages/release/bioc/html/genefu.html).

#### 乳腺癌整合生存分析网页工具
比较出名的是：http://kmplot.com/analysis/

The Kaplan Meier plotter is capable to assess the effect of 54,675 genes on survival using 10,461 cancer samples. These include 5,143 breast, 1,816 ovarian, 2,437 lung and 1,065 gastric cancer patients with a mean follow-up of 69 / 40 / 49 / 33 months. Primary purpose of the tool is a meta-analysis based biomarker assessment.

还有：http://co.bmc.lu.se/gobo/

CharacteristicsNumber of tumorsAll tumors1881ER+ tumors1225ER- tumors395Untreated tumors927TAM treated tumors326

