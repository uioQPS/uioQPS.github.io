# 【bioinfo】-Q&A-51

## 探索EMT的中间态

文章是：Identification of the tumour transition states occurring during EMT. Nature ; PMID: 29670281

整个文章基于两种老鼠模型：

- a genetic mouse model of skin squamous cell carcinoma (SCC)
- K8CreER/Pic3caH1047R/p53fl/fl and MMTV-PyMT breast tumours

### 摘要

在癌症中，上皮-间充质转化（EMT）与肿瘤干细胞的转移和对治疗的抗性有关。最近有人提出，**EMT不是一个二元的过程，而是通过不同的中间状态发生。**然而这个设想没有直接的体内证据。本次研究中，我们在皮肤和乳腺原发肿瘤中筛选出大量的细胞表面标志物，并鉴定出不同EMT阶段相关的多个肿瘤亚群的存在：从完全上皮化到完全间充质细胞状态以及中间混合状态。虽然所有的EMT亚群呈现相似的肿瘤增殖细胞能力，但它们在**细胞可塑性、侵袭性及转移潜能等方面均表现出差异**。他们的转录和表观遗传表现识别潜在的基因调控网络、转录因子和信号通路，控制不同的EMT过渡状态。最后，这些肿瘤亚群分布在不同的位置，对EMT过渡状态有不同的调节作用。

### EMT 背景知识
>EMT，全称epithelial–mesenchymal transition，又称翻译为上皮间质转换，指的是上皮细胞在一些因素的作用下,失去极性及细胞间紧密连接和黏附连接, 获得了浸润性和游走迁移能力, 变成具备间质细胞形态和特性的细胞的改变。这种行为是可逆的。

EMT的概念最早是1982年由Green-berg和Hay提出。然而，长久以来，科学界对于EMT在肿瘤转移过程中的作用一直存在争议，主要是因为无法在体内观察EMT过程 。

胚胎发育与癌症发展中的细胞可塑性变化有着惊人的相似性，而这种可塑性变化受到上皮间质转化epithelial-mesenchymal transition （EMT）过程的调节。胚胎发育时期，上皮状态和间充质状态的细胞能够自由转化。

- 上皮间质转化（EMT）使得细胞具备转移和浸润特性。
- 其反向过程，间质上皮转化mesenchymal-epithelialtransition （MET）赋予了细胞极性变化并失去移动能力。

EMT会促发癌细胞从病灶分离，转移到其它部位，而MET导致癌细胞停留，并在停留处引起新的肿瘤。

在很长一段时间内，**许多科学家都认为EMT过程对于肿瘤转移是一个必须条件**，EMT过程能够剥夺细胞与邻近细胞紧密结合的能力，使细胞能够在全身范围内迁移。科学家们认为是EMT给了癌细胞“双腿”让它们能够脱离原位肿瘤，转移到其他部位形成肿瘤转移灶。

#### 一、 首先介绍下，EMT发生常见的标志分子。

1.表达减少：E- cadherin，Cytokeratin，ZO-1。

2.表达增多：N- cadherin，Vinmentin，Snail1，Snail2，Twi st，MMP-2，MMP-3，MMP-9。

3.活性增加：ILK，Rho。

#### 二．参与EMT过程控制的常见信号通路有：

1.TGF-β信号通路。

2.Wnt信号通路。

3.Notch信号通路。

4.SMAD信号通路。

5.PI3K-AKT-MTOR信号通路。

#### 三. EMT过程有着复杂的调节网络：

表观遗传的调节

转录调节

选择性剪接

蛋白质的稳定

亚细胞定位

这些调节方式多样性使得EMT的调节往往不是线性的。此前，EMT调节过程中关于E-cadherin的转录，EMT转录因子SNAI1，SNAI2， ZEB1，ZEB1和 TWIST1等研究较多。

### 上皮细胞黏附分子（EpCAM）
与普通血细胞相比,CTCs具有某些特殊的生物标志物,包括CTCs表面的上皮细胞粘附分子(Epithelial cell adhesion molecule,EpCAM)

不同文献报道不一致：

- 在咽癌中Ep-CAM高表达与淋巴结转移密切相关，Ep-CAM表达越高，淋巴结转移发生频率越高
- 在头颈部鳞状细胞癌中，Ep-CAM在转移灶中表达比原发灶更高
- 在胃癌骨髓转移灶中的播散性肿瘤细胞Ep-CAM的表达较原发灶降低

免疫组织化学中的细胞角蛋白(Cytokeratin, CK) :

### 存在不同的EMT中间态
2017年的CELL文章做了scRNA-seq of human SCCs，Single-Cell Transcriptomic Analysis of Primary and Metastatic Tumor Ecosystems in Head and Neck Cancer 发现了EMT的过渡状态：






#### 数据下载
单细胞针对的是 scRNAseq of YFP+ Epcam+ and Epcam- skin squamous cell carcinoma cells使用的是 SMART-seq建库方法，384孔板，共383个单细胞数据。

- GSE110587 (RNA-seq and ATAC-seq)
- GSE110357 (scRNA-seq)

其实bulk测序包括：

GSE110584Identification of the tumour transition states occurring during EMT [ATAC-seq]GSE110585Identification of the tumour transition states occurring during EMT [RNA-seq]GSE110586Identification of the tumour transition states occurring during EMT [p63 overexpression RNA-seq]

作者在GEO里面同时上传了自己分析好的表达矩阵，所以可以下载表达矩阵模仿一下作者的下游分析，比如如何找marker及分类。

#### 单细胞把marker重新分类
PCA分析可以看到第一主成分就是用来区分E和M的，解释度高达 21%，而构成这些解释的基因可以可视化看它们的贡献度。






把EMT相关基因可以分成3个状态 Epithelial, hybrid and mesenchymal ：

>pure epithelial genes Esrp1 and Ovol1
hybrid epithelial genes Krt5 and Trp63
hybrid mesenchymal genes Prrx1, Snai1, Zeb1, Zeb2, Pdgfra and Col3a1
pure mesenchymal genes Aspn and Mmp19

#### 单细胞转录组聚类结果
这5个cluster非常重要，是全文的重点，






#### 实验摸索EMT过程的5个形态
癌症基础知识背景不强，这些实验没有看懂，大概应该是有着不同CD分子marker的那些细胞群体表现很不一样吧。

默认知识点： epithelial YFP+Epcam+ and mesenchymal-like YFP+Epcam− tumour cells (TCs)8

- CD61 (also known as Itgb3)
- CD51 (also known as Itgav)
- CD106 (also known as Vcam1)





#### 使用ATAC-seq数据来探索不同EMT中间态
Transcription factor motifs enriched in the ATAC-seq peaks that were upregulated between the indicated subpopulations as determined by Homer analysis using known or de novo motif search (asterisk) or using JASPAR motif matrix (hash symbol).






Transcription factor motifs enriched in the ATAC-seq peaks that were upregulated between the indicated tumour subpopulations as determined by Homer analysis using known or de novo motif search (asterisk).

Green, core transcription factors; pink, epithelial transcription factors; yellow, mesenchymal transcription factors. c–j, Analysis based on ATAC-seq results from different subpopulations derived from one tumour.

