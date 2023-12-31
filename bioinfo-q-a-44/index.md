# 【bioinfo】-Q&A-44

## 用LC-MS来找HCC的代谢标记物

### 代谢组学简介
代谢组学的概念来源于代谢组，代谢组是指某一生物或细胞在一特定生理时期内所有的低分子量代谢产物，代谢组学则是对某一生物或细胞在一特定生理时期内所有低分子量代谢产物同时进行定性和定量分析的一门新学科。

**代谢物水平整合了来自内源性物质（基因）和外源性物质（营养、药物、环境）的信息。**对一组代谢物进行并行定量采集可以直接获得细胞和生物系统的生化状态。代谢轮廓分析与生物表型相关，可以用于生物标志物的发现和路径分析。代谢组学需要敏感和快速的分析方法，以及阐述了生物信息学处理/分析的数据。

代谢组学使用的分析策略有三种：

- 非靶向代谢组学
- 半靶向代谢组学
- 靶向代谢组学

### LC-MS背景介绍
**液质联用(LC/MS)**: LC为液相色谱仪；MS为一种能够生成离子，在气态中根据质荷比的不同将其分离并进行检测的仪器。*LC/MS以液相色谱作为分离系统，质谱作为检测系统，*因而兼具有液相色谱高分离度与质谱高灵敏度的特点。分析的样品在质谱部分和流动相分离，被离子化后，经质谱的质量分析器将离子碎片按质量数分开，经检测器得到质谱图。

LC-MS/MS越来越广泛应用于药物、食品、环境、法医、临床等各个领域。尤其在临床检测上适，用范围远远超过放射性免疫检测和化学检测范围，是其他方法无可比拟的。

质谱本身也有一个分离作用，就是按照质量的分离，如果液相分离了一次，那LC－MS就分离了两次，而LC-MS/MS就分离了三次，LC-MS3就分离了四次....（3级以上是离子阱质谱的特点）。

质谱定量中已经被广泛接受的方式是MS/MS定量。这种定量常通过三级四极杆或离子阱质谱实现。要求使用MS/MS的原因是：许多化合物有同样的质量。当使用第一个维度即单级质谱MS去定量时，也会缺乏特异性，尤其是对于像血液那样的复杂的基质。第二个维度的MS（即MS/MS）在大多数情况下，能够提供唯一的断裂。合并特异的母离子质量和唯一的碎片离子信息，可以选择性地监测被定量的化合物。

### 技术进步
#### 3min方法测定400多种代谢物
科罗拉多大学丹佛分校Angelo D'Alessandro教授课题组2017年发表在Rapid Communication in Mass Spectrometry上的一篇文章，题为A three-minute method for high-throughput quantitative metabolomics and quantitative tracing experiments of central carbon and nitrogen pathways。Angelo D'Alessandro教授课题组基于Vanquish-Q Exactive 技术建立了3min方法测定400多种代谢物的方法，覆盖到糖酵解途径、磷酸戊糖途径、三羧酸循环、鸟氨酸循环、氨基酸代谢途径、嘌呤代谢及其嘧啶代谢等多种和癌症相关性较强的代谢途径，且适用于体液、血浆、组织、细胞等复杂基质，极大的提高了复杂基质中代谢物研究通量，并实际应用于潜在标记物发现、代谢流分析、发病机理研究等领域，助力于精准医学研究、转化医学及其临床代谢组研究工作。

#### pseudotargeted metabolomics 伪靶向代谢组学
既可以保证检测到足够多的化合物，又可以保证其定量的准确性
```
工作流程：
使用高分辨质谱获得化合物的二级质谱。采用DDA扫描（又称作auto-MS/MS）在非靶向的情况下，获取化合物的二级质谱。
挑选离子对，以供下一步MRM分析。通常用仪器自带软件，去寻找母离子→子离子离子对，在这一步需要仔细查看所选择的离子对，因为离子对的挑选直接影响下一步的MRM分析。
使用三重四级杆质谱对第二步挑选的离子对进行扫描，积分峰面积。
数据分析。
```

### 文章的实验设计
文章的团队从2008年9月到2014年5月，共招募了1448人，分别是下面6种：
```
normal controls (healthy volunteers, NC)
patients with chronic hepatitis B (CHB) infections
liver cirrhosis (Cir)
HCC
gastric cancer (GSC)
intrahepatic cholangiocarcinoma (ICC)
```

确定仅仅由2个代谢物组成的panel
对数据进行下面的分析：

Ultimately, eight metabolites were retained: phenylalanyl-tryptophan (Phe-Trp); glycocholate (GCA); taurocholate; lysophosphatidyle-thanolamines 18:2, 20:5, and 22:4; choline; and taurine.
Subsequently, a binary logistic regression analysis and an optimized algorithm of the forward stepwise (Wald) method were employed to construct the best model using these eight potential metabolite bio- markers.
Finally, the combination of Phe-Trp and GCA was defined as the ideal biomarker panel to distinguish patients with HCC from subjects without HCC.


评价panel的表现
也就是一些最基础的统计分析，把代谢组学数据结合病人的临床信息进行数据统计即可。



代谢组数据分析
这篇文章使用的是 SIMCA-P （http://umetrics.com/products/simca），该软件是由Umetrics公司在1987研究开发，目前是一款公认的多元变量统计分析软件，被绝大多数代谢组服务提供商所采用。SIMCA-P虽然是一款强大的多元变量统计分析软件，但也有不足之处：

SIMCA-P是一款商业软件，需要收费（但有30天免费试用），windows版本的。对于想使用免费软件的老师来说，不那么友好；
代谢组数据的分析除了多元变量统计分析，还有原始数据前处理（pre-processing）、数据处理（processing）、单变量统计分析等，很显然SIMCA-P并不能满足我的所有愿望
在《Metabolomics-Fundamentals and Applications》这本书的第四章节《Processing and Visualization of Metabolomics Data Using R》中，作者列出了一些代谢组数据处理、统计分析与可视化的开源免费软件工具！代表性的如下：

MetaboAnalyst是2009年公布的一款代谢物数据分析软件
XCMS Online（https://xcmsonline.scripps.edu）是XCMS的网页版本
XCMS是2006年发布的、基于R语言的用于LC-MS数据处理分析的软件


检测到表现差异的代谢物非常多
```
DMG, N,N-dimethylglycine;
GPC, Glycerophosphate choline;
DHC, dihydrocholesterol;
Phe-Phe, PhenylalanylPhenylalanine;
Phe-Trp, Phenylalanyltryptophan;
DMHC, dimethylheptanoylcarnitine;
ANDS, androsterone sulfate;
DHTS, dihydrotestosterone sulfate;
TCA, taurocholate;
GCA, glycocholate;
GCDCA, glycochenodeoxycholate;
TCDCA, taurochenodeoxycholate.;
GCDCS, glycoursodeoxycholate sulfate;
ACN, acetylcarnitine;
LPC, lysophosphatidylcholine;
LPE, lysophosphatidylethanolamine;
SM, shpingomyelin;
PC, phosphatidylcholine;
FFA, free fatty acid;
MG, Monoacylglycerol,
FAAD, fatty amide.
```

