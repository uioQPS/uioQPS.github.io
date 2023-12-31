# sdAb panning



## 高通量筛选G蛋白偶联受体的功能性抗体

combining glycosylphosphatidylinositol-anchored antibody cell display with β-arrestin recruitment-based cell sorting and screening.将糖基磷脂酰肌醇锚定抗体细胞展示与基于 β-arrestin 募集的细胞分选和筛选相结合。

抗体基因型与表型；GPI 和 GPCR 在脂筏中的共定位促进了 GPI 锚定抗体与 GPCR 靶点的相互作用

免疫抗体库中鉴定出一组针对人类 apelin 受体的抗体拮抗剂和抗体激动剂来验证该方法。
相比之下，噬菌体展示仅从相同的库中获得了中性结合剂和抗体拮抗剂


```
sdAb
GPI-anchored sdAb library (1 × 108 cells）
apelin 13: 100nM 激动剂
460nm 530nm代表
产物/substrate：越低越好
```

### 1、APJ PathHunter assay
阳性clone的可溶性sdAbs有抑制剂活性

### 2、PathHunter β-arrestin reporter assay
不适合筛选表面抗体的细胞文库，洗完之后没有fluorescent substrate for the reporter enzyme nor the product resides in the cells

### 3、Tango/APJ β-arrestin assay 
β-内酰胺酶报告酶水解前后细胞中保留荧光底物

POC study：APJ gene is stably integrated into the genome, as well as a transcription factor linked to the C-terminus of APJ through a tobacco etch virus (TEV) protease site. 

细胞稳定表达β-arrestin and TEV protease fusion protein

```
转录因子的切割
随后的 β-内酰胺酶报告基因的表达
β-内酰胺酶介导的酶促反应
```
### 4、LANCE cAMP assay（antagonist activity）
2000 cells in 10 μl assay buffer (HBSS + 5 mM HEPES + 0.1% BSA) were added into each well on a 96-half well assay plate and stimulated with test antibodies (at agonist mode), or with apelin 13 ligand at EC80 dose together with test antibodies (at antagonist mode) in the presence of 2.5 μM forskolin. 

-----

### Proof of concept study and generation of a GPI-anchored sdAb library.

pHBLV-puro-GPI 慢病毒表达载体 Covalently linked DAF allows his-tagged sdAbs to anchor to GPI through a flexible linker；共价连接DAF让sdAb锚定到GPI

概念验证 (POC) 研究，APJ 中性结合 sdAb JN126、激动性抗体-配体融合 JN126P13、拮抗性 sdAb JN241 和一个对照 sdAb E3，JN126 和 JN241 是先前通过噬菌体展示从 APJ 免疫文库中分离出来的 APJ sdAb，而 JN126P13 是与 apelin 13 共价连接到中性结合 sdAb JN126 的 C 末端的抗体-配体融合物。

产生重组慢病毒和转导稳定表达 CHO-k1 衍生的 PathHunter β-arrestin 报告细胞的 APJ 后，我们通过流式细胞术测量抗体表面表达水平，并通过 β-arrestin 募集测定测试转导细胞的生物学功能。

磷脂酰肌醇磷脂酶 C（PI-PLC）处理这些细胞显着降低了这些抗体的表面密度，证实它们确实锚定在 GPI 上（sup1c）

在 PathHunter β-arrestin 测定中，GPI 锚定的 sdAb JN241 和抗体-配体融合 JN126P13 被证实分别是拮抗剂和激动剂，而中性结合 sdAb JN126 和对照 sdAb E3 是无功能的（sup1d，e）

a)功能实验证明 all four tested sdAbs behave the same as that in APJ PathHunter reporter cells

b)cells with antagonistic antibodies anchored on the cell surface 

c)cells with agonistic antibodies anchored on the cell surface

![](https://media.springernature.com/full/springer-static/image/art%3A10.1038%2Fs42003-020-0867-7/MediaObjects/42003_2020_867_Fig1_HTML.png)

丰富 APJ 特异性 sdAb 克隆: 293FT/huAPJ 细胞对噬菌体展示的免疫文库进行了一轮淘选，然后将插入物从淘选文库批量转移到慢病毒载体 pHBLV-puro-GPI。 我们在 pHBLV-puro-GPI 中构建了一个 GPI 锚定的 sdAb 文库，大小为 4×107(表达cell-displayed sdAb library可用anti-his antibody)

### Function-based cell sorting and screening for sdAb antagonists.

#### antagonist activity

![](https://media.springernature.com/full/springer-static/image/art%3A10.1038%2Fs42003-020-0867-7/MediaObjects/42003_2020_867_Fig2_HTML.png)

分选cell： low product intensity at 460 nm and high substrate intensity at 530 nm 

cells with antagonist activity were enriched based on the dramatically decreased ratio of product/substrate intensity


127 single-cell clones were subsequently tested for antagonist activity by **β-arrestin recruitment assay**. In all, 104 out of 127 cell clones showed higher than 20% inhibition.

阳性细胞单克隆RT-PCR得到抗体基因测序

IC50测定

the percentage inhibition of a cell clone correlates well with the IC50 of soluble sdAb isolated from the corresponding cell clone 细胞克隆的抑制百分数和分离得到的可溶sdab的IC50高度相关

### Sequence and epitope comparison of isolated APJ sdAb antagonists. 
IC50s obtained by the cAMP assay correlate with those by the β-arrestin recruitment assay

与主要与 ECL2 结合的噬菌体展示衍生的 sdAb 拮抗剂不同，通过基于功能的细胞分选和筛选分离的 sdAb 拮抗剂与 APJ 上的不同表位结合，可分为五组; 其余两组具有中度至弱拮抗剂活性的组与 APJ 的 N 端和几乎所有三个细胞外环（第 4 组和第 5 组）结合，表明它们对受体的构象完整性更敏感。

![](https://media.springernature.com/full/springer-static/image/art%3A10.1038%2Fs42003-020-0867-7/MediaObjects/42003_2020_867_Fig3_HTML.png)

### Function-based cell sorting and screening for sdAb agonists.
β-arrestin recruitment-based cell sorting for the cells with high product intensity (signal at 460 nm) and low substrate intensity (signal at 530nm) following the reporter substrate loading

![](https://media.springernature.com/full/springer-static/image/art%3A10.1038%2Fs42003-020-0867-7/MediaObjects/42003_2020_867_Fig4_HTML.png)
72 cell clones were screened by Tango/APJ β-arrestin assay at agonist mode

One sdAb agonist, designated JN300

agonist activity was confirmed with the presence of the product (in blue color)

可溶性 sdAb JN300 的结合和激动剂活性。 JN300 与人类 APJ 表达细胞特异性结合。测试其与野生型 (WT) APJ、APJ E174A 突变体和 APJ/DOR **环交换嵌合体** 的结合来定位 JN300 的表位。我们发现对 N 末端或三个 ECL 环的任何变化都会显着破坏 JN300 与 APJ 的结合，这表明 APJ 受体的结构完整性对于 JN300 介导的受体激动是至关重要的。

在 β-arrestin、cAMP 和基于 PathHunter 的 APJ 内化试验中，JN300 的半数最大有效浓度 (EC50) 为 80-90 nM。我们在氨基酸序列和表达水平上将 JN300 与分离的十种 sdAb 拮抗剂进行了比较, JN300除了JN300的CDR1较短外，在V、D、J基因使用和CDR环长度方面没有表现出任何特殊特征，但JN300在293F瞬时表达系统中的产量低于10 种 sdAb 拮抗剂。我们目前正在尝试共结晶 APJ 和 JN300 复合物。复杂结构的解析可以揭示 JN300 介导的受体激动作用。
![](https://media.springernature.com/full/springer-static/image/art%3A10.1038%2Fs42003-020-0867-7/MediaObjects/42003_2020_867_Fig5_HTML.png)

### Overview
一种将 GPI 锚定抗体细胞表面展示与 β-arrestin 募集报告基因分析相结合来有效鉴定针对 GPCR 的抗体拮抗剂和激动剂的新方法。

在抗体基因扩增和亚克隆之前，可以通过直接用PI-PLC消化GPI锚定抗体获得可溶性抗体，快速确认生物功能，进一步减少工作量，加快功能鉴定过程。

基于 β-抑制蛋白募集的细胞分选和筛选，而不是监测基于 G 蛋白的信号传导。基于β-抑制蛋白募集的细胞分选和筛选的优势在于它不依赖于对靶受体的G蛋白信号特异性的了解。

APJ 作为模型 GPCR 靶标来验证该方法，并确定了一组具有不同表位的 APJ sdAb 拮抗剂，并且据我们所知，这是第一次来自免疫 sdAb 文库的 sdAb 激动剂。

sdAb 拮抗剂和非功能性结合剂分别主要结合 ECL2 和 APJ 的 N 端。 APJ ECL2 对配体结合至关重要，并在介导配体诱导的受体激活中起重要作用。可以理解的是，sdAbs 与 ECL2 的结合阻断了 apelin 13 与配体结合口袋的结合，从而产生竞争性拮抗剂活性，而 sdAbs 与 N 端的结合不影响配体结合。

**something to improve：**

- 大型噬菌体展示的抗体文库转移到慢病毒载体中之前使用了一轮噬菌体文库淘选来丰富结合物而不失去多样性。引入一轮基于结合的噬菌体文库淘选可能会导致激动性克隆的丢失，只发现了一种 sdAb 激动剂。
- GPI 锚定的激动剂抗体对细胞的组成性激活导致细胞中激动剂抗体基因的丢失甚至细胞凋亡，这导致基于功能的激动剂 sdAb 基因无法恢复。
- 细胞分选和筛选。也有可能在分选过程中细胞密度高时，细胞被相邻细胞展示的激动剂抗体激活，这可能导致假阳性信号。 GPI 锚定抗体的诱导表达可以减轻这种负面影响并提高分离激动剂抗体的分选和筛选效率。

APJ sdAb 激动剂 JN300 是迄今为止发现的第一个针对 A 类 GPCR 的抗体激动剂，A 类 GPCR 是用于药物发现的最大和最重要的 GPCR 家族。 JN300 可治疗慢性心力衰竭。由于 APJ 也参与血管生成，并且在实体瘤中观察到 APJ 升高，因此本研究中确定的 APJ sdAb 拮抗剂可能具有通过阻断肿瘤血管生成来治疗实体瘤的潜力。

这种基于功能的抗体高通量筛选方法可应用于所有 GPCR 靶标。它还可以通过使用监测下游信号通路的报告基因检测来适应分离针对非 GPCR 的功能性抗体。我们相信这里描述的新方法将极大地促进具有治疗潜力的 GPCR 功能抗体的鉴定，这可能为抗体治疗创造新的范例。

