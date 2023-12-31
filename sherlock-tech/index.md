# SHERLOCK tech


## SHERLOCK技术

### 什么是SHERLOCK技术
SHERLOCK技术，全称是Specific High Sensitivity Enzymatic Reporter UnLOCKing，由华裔科学家张锋研发的一种新的快速，便宜，高灵敏度的诊断工具，相关文章发表在2017年的Science上[1]。SHERLOCK技术可以用于检测血清、尿液和唾液中低浓度的病毒，灵敏度可达阿摩尔级（aM，10的负18次方摩尔每升），而且可以用于检测病毒载量，区分不同的菌株，甚至可以分辨含有不同耐药突变的菌株和不同的癌症菌株突变，目前已经成功应用到武汉新型肺炎的检测中，具体的操作步骤已经公布在张锋课题组的网站上 (A protocol for detection of COVID-19 using CRISPR diagnostics)。

### SHERLOCK技术原理
SHERLOCK技术的原理由两部分组成，分别是RPA （Recombinase Polymerase Amplification）扩增和CRISPR-Cas13a系统。RPA是一种高灵敏度的等温扩增技术，在常温下即可完成核酸的快速扩增；CRISPR-Cas13a作为检测目的核酸序列的手段。下面将具体介绍这两个技术。
#### RPA
核酸扩增，可能大部分人接触到的仅有PCR（polymerase chain reaction），这说明你的知识该更新了。除了PCR，目前已有的核酸扩增技术还有RPA，NASBA（nucleic acid sequence-based amplification），SDA（strand displacement），RCA（rolling circle amplification），LAMP（the loop-mediated isothermal amplification），HAD (helicase-dependent amplification)，HCR (hybridization chain reaction)等技术
Detection，感兴趣的可以看相关的综述[2-4]。
RPA, Recombinase Polymerase Amplification，即重组酶聚合酶扩增技术。这是一种可以媲美PCR的技术，2006年由英国科技公司TwistDx发明。常规的PCR技术对温度控制有着复杂的要求，经历高温变性、低温复性还有中温延伸三个步骤，RPA就不需要这么麻烦，它的反应需要二种**在常温下有活性的酶（重组酶，聚合酶），最适温度在25-42℃之间，不需要变性过程，这样就大大加快了扩增的速度，再加上不需要温控设备，这两点共同打造了真正便携式的快速DNA扩增技术**。不仅方便快捷，RPA更“迷人”的地方在于**扩增量**十分高，能够将痕量的核酸（尤其是DNA）模板扩增到能够可以检测到的水平。痕量，顾名思义就是“该物质虽然存在，但只有‘痕迹’，无法用一般方法定量检测，通常含量＜0.1mg”。利用它可以从单分子得到大约1012的扩增产物。而且RPA还不需要复杂的样品处理，可以直接用病人的血清完成检测。
RPA的具体过程如Figure 1所示，涉及三个核心蛋白：recombinase, SSB, and strand-displacing polymerase。 首先，重组酶和引物结合形成复合物，在拥挤环境下利于复合物的形成。然后重组酶-引物复合物在目标DNA上寻找同源区域，找到之后会使得双链DNA解聚并形成D-loop。接下来SSB（单链结合蛋白）会结合到暴露的一条DNA链上，稳定D-loop，防止分支迁移。最后，聚合酶会使得重组酶与引物分离，并启动后续的扩增过程。整个过程中，**recombinase, SSB, and strand-displacing polymerase还有引物可以被反复利用，在循环过程中，复制产物以指数形式增长，直到体系中的dNTP消耗完。**

![](1.png)
Figure1：RPA cycle

#### CRISPR-Cas13a
Cas13a蛋白，又称C2C2蛋白，它的发现历程比较有意思。2015年，张锋实验室通过序列比对发现Cas13a与众不同，跟cas9相比，其不具有Ruvc结构域，即没有DNA酶活性，但是具有两个HEPN结构域，即具有RNA酶活性，这一结论在2016年4月的Science杂志上得到了验证，Cas13a是一个RNA介导的RNA酶（figure2a）。然而，过个短短几个月之后，张锋的竞争对手Jennifer Doudna教授更进一步，在nature上发表文章，证实证明Cas13a具有两种RNA酶活性，一种是切割pre-crRNA称为成熟crRNA酶活性，一种是HEPN结构域的RNA酶活性，这个怎么理解呢，简单来说就是Cas13a除了能够切割crRNA之外，还可以切割其遇到的任何RNA，即所谓的**“collateral effect”**，当然**后一种RNA酶活性需要crRNA识别相应的RNA序列后才能激活**。基于这一点，可以通过猝灭荧光的RNA作为报告基团来检测目的RNA（figure2b），但是当时Doudna教授实验室的检测灵敏度只能够达到nM级别，没有实际应用价值。

![](2.png)
Figure2：Cas13a complex and collateral activity

#### SHERLOCK的诞生
前面提到过，CRISPR-Cas13a是不能被称为“SHERLOCK”的，因为Cas13a是达不到许多基础研究和临床应用所需要的灵敏度的。于是，张锋教授找了一个强有力的“外援”——RPA。利用RPA技术，在室温条件下，样品中的DNA或RNA含量可以快速得到提升，然后再将DNA转化成RNA。如此一来，Cas13a就进化成真正意义上的“SHERLOCK”了！ SHERLOCK其实是一个“团队”，张锋给Cas13a“配备”了两个助手：一个是引导它到达应该被切割的特异性RNA的“向导RNA”；另一个叫做“报告RNA”，因为Cas13a在切割完特定RNA后不会失活，所以在Cas13a完成任务开始“作乱”的时候，报告RNA就会被切断，发出荧光信号。这样研究人员们就知道，它已经找到了与其相匹配的RNA序列（Figure 3）。

![](3.png)
Figure3：The scheme of SHERLOCK

### SHERLOCK实验设计
主要包括样品处理，引物设计，crRNA设计以及结果检测等步骤。
#### 样品处理
样品中的RNA酶会对检测结果带来极大的干扰，RNA酶导致向导RNA的讲解会产生假阴性的结果，RNA酶导致报告RNA的降解会带来假阳性的结果，为了避免这种干扰，在纯化核酸的过程中都需要加入RNA酶抑制剂。目前RPA已经分别有针对RNA提取和DNA提取的商业化试剂盒，保证结果不受RNA酶的影响。
#### 引物设计
RPA扩增的引物一般为30-35 nt，包含正向和反向这一对引物，其中正向引物5’端通常含有T7启动子区，便于扩增之后的转录过程（Figure 4）。引物的退火温度在54-67℃之间，扩增的总长度在80-400 bp。
#### crRNA设计
向导RNA设计，最关键的一点在于不能与引物有互补，因为crRNA与引物的结合可能会导致与扩增产物的脱靶。间隔序列（spacer sequence）可以与扩增产物的任意区域配对，间隔序列要避免形成二级结构。直接重复序列（direct repeat sequence）位于间隔序列的5’端。间隔序列和直接重复序列共同构成crRNA，如Figure 4所示。crRNA根据正义链来设计，可以通过化学合成或者转染质粒的方式来获得。

![](4.png)
Figure 4：The design of primer and crRNA

#### 结果检测
目前常用的检测手段主要有荧光检测和层析检测。荧光检测是通过在一段寡聚RNA两端分别耦联荧光基团和淬灭基团来实现的，当含有目的产物是，报告RNA会被Cas13a切断，从而使体系内荧光基团发出荧光（Figure 5a）。层析检测则是在报告RNA的两端分别耦联荧光素和生物素，试纸条上的链霉素可以结合生物素，荧光素抗体可以结合荧光素，这样经过层析之后通过条带可以判断检测结果（Figure 5b）。

![](5.png)
Figure5 ：Fluorescence detection and Lateral flow detection[9]

### 总结与展望
SHERLOCK技术具有以下的优点：1）高灵敏度：灵敏度可至aM级别，即1ul样品里的单个RNA或DNA分子都可以检测出来；2）高特异性：由于crRNA需要与目的核酸完全配对才会激活cas13a的RNA酶活性，所以单核苷酸突变也可以检测出来；3）快速：整个检测可以在1h内完成；4）成本低廉，一次检测的价格大约在0.6美元；5）便携：不需要借助PCR等精密的仪器，在常温下就可完成。凭借着其独特的优势，SHERLOCK技术有望在Point-of-care领域大放异彩。

基于RPA技术和CRISPR技术的结合还有很多可以挖掘的地方，基于Cas13a特性开发出的SHERLOCK是其中一个很好的例子，Cas13a的“collateral effect”是实现检测的前体，在Cas蛋白庞大的家族里面，我相信也会发现更多的有意思的酶用于核酸检测。




**参考文献：**
```
[1] Jonathan S. Gootenberg, Omar O. Abudayyeh, Jeong Wook Lee, et al. Nucleic acid detection with CRISPR-Cas13a/C2c[J]. Science 4(2017) 438-442.
[2] L. Yan, J. Zhou, Y. Zheng, et al, Isothermal amplified detection of DNA and RNA[J]. Mol. Biosyst. 10 (2014) 970–1003.
[3] P. Craw, W. Balachandran, Isothermal nucleic acid amplification technologies for point-of-care diagnostics: a critical review[J].Lab Chip. 12 (2012) 2469–2486.
[4] O. Piepenburg, C.H. Williams, D.L. Stemple, et al, DNA Detection Using Recombination Proteins[J]. PLoS Biol. 4 (2006) 1115–1121.
[5] Ivan Magriñá Lobato, Ciara K. O’Sullivan. Recombinase Polymerase Amplification: Basics, applications and recent advances[J]. TrAC Trends in Analytical Chemistry. 1(2018) 19-35.
[6] Rana K. Daher, Gale Stewart, Maurice Boissinot. Recombinase Polymerase Amplification for Diagnostic Applications[J]. Clinical Chemistry. 7(2016) 947-958.
[7] Abudayyeh O O, Gootenberg J S, Konermann S, et al. C2c2 is a single-component programmable RNA-guided RNA-targeting CRISPR effector[J]. Science. 8(2016).
[8] East-Seletsky A, O’Connell M R, Knight S C, et al. Two distinct RNase activities of CRISPR-C2c2 enable guide-RNA processing and RNA detection[J]. Nature. 9(2016) 270-273.
[9] Max J. Kellner, Jeremy G. Koob, Jonathan S. Gootenberg, et al. SHERLOCK: nucleic acid detection with CRISPR nucleases[J]. Nature protocols 9(2019) 2986–3012.
```

