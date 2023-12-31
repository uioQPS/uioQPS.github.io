# 【bioinfo】-Q&A-53

## 多靶点自体免疫细胞技术

#### 过继性T细胞治疗(adoptive T-cell therapy,ACT)
文章是 Immune recognition of somatic mutations leading to complete durable regression in metastatic breast cancer 发表于 Nature Medicine (2018) 重点在实验设计环节，但是现在的大文章或多或少都引入多组学数据，本文也不例外，实验纳入的唯一一个病人既做了全外显子又做了转录组测序，还有单细胞测序。不过数据公开的是 PRJNA342632 for exome data and PRJNA243084 for RNA-seq data

#### 背景知识
#### 肿瘤免疫疗法
近年来，肿瘤免疫疗法已成为继手术、放疗和化疗之后的第四种肿瘤治疗手段。

**肿瘤免疫治疗**，主要包括
```
免疫疫苗
免疫检查点抑制剂治疗
过继性免疫细胞治疗
细胞因子治疗
其中以CTLA4、PD1/PDL1抑制剂为代表的免疫检查点抑制剂，以其显著的临床疗效而备受瞩目。
```
而本文的研究属于肿瘤免疫治疗的过继性T细胞疗法（ACT）领域，之前以CART为主，最近出现了另外一种疗法——TIL（Tumor Infiltrating T cell，肿瘤浸润淋巴细胞），表现也十分惊艳，Rosenburg博士及其团队就是这一领域的代表。

#### 肿瘤浸润淋巴细胞(TILs)
肿瘤浸润性淋巴细胞（tumor infiltrating lymphocyte, TIL）指的是从肿瘤组织中分离出的浸润淋巴细胞。

1986年，Rosenberg研究组首先报道了TIL细胞。TIL细胞表型具有异质性。一般来说，在TIL中，绝大多数细胞是CD3阳性的。在不同肿瘤来源的TIL细胞中，CD4+ T细胞、CD8+ T细胞的比例有差异，但大多数情况下以CD8+T细胞为主。

不同患者不同肿瘤类型对免疫疗法的反应不可预测，是因为免疫成分的异质性以及肿瘤浸润淋巴细胞（TILs）在单个肿瘤内和患者之间的多样化表型。

并不是所有的肿瘤浸润T细胞都是针对肿瘤抗原的，并且发现检测CD39的表达可以简单的量化或分离Bystander 即“旁观者”T细胞。

#### 技术历史
早在2014年的时候Rosenburg博士的团队就将其一部分研究结果发表在Science期刊（Tran E et al, Science 2014）上，当时他们的做法是：把一名晚期胆管癌女患者的肺转移肿瘤病灶拿来做全基外显子测序，找到了26个非同义突变，**并为每一个突变构建了一段短基因，将这些短基因几个一组地串联成短基因串（Tandem Minigene, TMG）**，然后将这些短基因串转入抗原递呈细胞（指树突状细胞DC）并与肿瘤浸润淋巴细胞（TIL）共同培养，以查看患者的TIL中是否存在可以特异性识别肿瘤突变的T细胞。不出所料，这位患者的TIL可以识别肿瘤的一个突变。研究者将这个有识别功能的T细胞体外大量扩增后再回输给患者，患者肝部的转移灶长期稳定状态达1年以上。Rosenburg博士的团队通过这个研究，证明了晚期癌症患者的肿瘤组织中确实是存在可以对抗肿瘤突变的淋巴细胞的、即便是在晚期胆管癌这样凶险又难治的瘤种。

但是从实体瘤中收集浸润性T细胞确实比较有挑战性，而与之相比，抽取患者的外周血则容易得多，所以2016年Steve Rosenberg小组发表在《自然-医药》杂志上的文章《Prospective identification of neoantigen-specific lymphocytes in the peripheral blood of melanoma patients》即是从晚期恶性黑色素瘤患者的外周免疫细胞中可根据PD-1受体表达找到针对肿瘤变异新抗原的活性杀伤T细胞。这样找到的T细胞在体外增殖后和肿瘤浸润淋巴细胞（TIL）一样可以识别患者自身肿瘤。

但是恶性黑色素瘤是一种突变比较多的肿瘤，尽管外周血抗肿瘤细胞比例低，但是仍然能够分离出来；而其他突变较少的肿瘤在外周血中是否同样存在这样的T细胞就存疑了。



#### 多组学测序
虽然作者本文只探索了一个病人，但是它上传了很多数据在，对这个病人测了：
```
FrTu (118×)
OCT (120×)
normal blood (116×)
```
也是常规的肿瘤外显子数据分析流程，其实就是GATK best-practices 流程加上 varscan2，再使用 ‘allele-specific copy-number analysis of tumors’ (ASCAT) 和Sequenza 来分析肿瘤纯度和倍性，然后得到的somatic 变异过滤如下：
```
tumor and normal read counts of 6 or greater,
variant-allele frequency (VAF) ≥ 5%
tumor-variant reads ≥ 3
```
其RNA-seq测序也是走的STAR比对加上GATK best-practices 流程来找变异。

最后使用 PyClone 进行克隆进化分析。

#### somatic变异
这里过滤后剩下 62 nonsynonymous somatic 变异位点

#### TIL识别的两个基因突变
通过与前面文章类似的方法，构建TMG来挑选病人那些可以特异性识别这些突变的TIL细胞。本次使用使用了pulsed peptide pools (PPs 1–6) or transfected mRNAs (tandem minigenes (TMGs) 1–6)

得到的结果如图：

- The reactive F13 TIL recognized mutant ECM29 proteasome adaptor and scaffold (ECPAS; also known as KIAA0368)
- Individual peptide screens of the TIL fragments showed F8 and F12 recognized the solute carrier family 3 member 2 (SLC3A2) protein





#### 单细胞测序识别两个基因突变
挑选那些高表达T cell receptor (TCR)-β variable (TRBV) region or high expression levels of 4-1BB (CD137, a marker of T cell activation)的T细胞做单细胞测序，找到了跟mutant (mut)-SLC3A2 or mutKIAA0368对接的TRBV

然后TIL回输之后， FACS-sorted TRBV28+ cells showed specific recognition of PP6 and TMG6，如下图






又对应到了两个突变：

- calcium-dependent secretion activator 2 (CADPS2; p.Arg1266His)
- cathepsin B (CTSB; p.Asp159His)

整个实验比较幸运，对这个病人来说，发现了 11 TCR clonotypes recognized the four neoantigens (SLC3A2, KIAA0368, CADPS2 and CTSB)

