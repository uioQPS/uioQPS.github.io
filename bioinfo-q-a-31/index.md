# 【bioinfo】-Q&A-31

## 第31题 RNA-Seq建库用哪种策略？
Hello 大家好！ 我们又见面了！

我们将与大家一同讨论与转录组建库，基因的差异表达分析等相关的内容。

今天我们讨论的是RNA-Seq建库策略问题。

### 什么是RNA-Seq建库
RNA-Seq建库简单来说，就是把我们想要进行转录组测序的样品（一盘细胞，若干片叶子等）提取RNA，并给RNA（主要是mRNA）序列加上adapter（接头）的过程。最终的目的是为了让转录组的片段两边都连上已知的adapter序列，方便之后的测序。结构如图1-1所示，图中所示的文库片段就是我们这里说的转录组片段。

在整个实验过程中，有几个关键的问题值得思考。

- 第1方面，最终拿去测序仪进行测序的一定都是DNA片段，因此我们需要对实验提取到的RNA序列进行反转录成cDNA，然后再进行连接adapter。不过这个问题不算难，因为只要有RNA的序列，就可以使用random primer进行随机反转录，获得对应的cDNA片段。而在cDNA片段上链接adapter就是标准的DNA连接反应，非常简单。

- 第2方面，细胞中含量最多的是rRNA，其次是tRNA，mRNA与lncRNA只占大约5%左右甚至更低，而我们真正关心的往往就是mRNA与lncRNA代表的基因表达量。那么怎么对mRNA或者是lncRNA进行富集呢？这就是我们今天要跟大家强调的富集策略问题。

###  PolyA + 与 rRNA -两种富集策略
一般来说，我们做RNA-Seq的假设是基于：
```
某一个条件影响了表型 -->
假设，表型是由蛋白的变化引起的 -->
假设，蛋白的变化大概率是由对应gene表达量的改变引起的 -->
因此，需要对gene进行定量 -->
最终，进行RNA-Seq
```
#### 1 polyA + 建库策略

也正是因为上述的推理过程，我们才需要去做RNA-Seq对全转录组进行gene的定量分析。在绝大多数情况下，我们关心的gene都是protein coding gene或者是与其结构非常详细的lncRNA（long non-coding RNA）这些gene在形成成熟的mRNA以后，往往都会在3’末端带有polyA 尾巴，如图2-1所示。同时，成熟的tRNA，rRNA，miRNA等等都不带有经典的polyA尾巴。

因此，如果我们使用一段 带有能与A配对的T的磁珠就可以与对应的带有polyA尾巴的RNA结合，随后再经过**随机打断，反转录，加adapter**就可以完成建库，这样的建库策略叫做polyA + 或者 polyA positive。核心是对带有polyA尾巴的RNA进行富集。

#### 2 rRNA -建库策略

那么polyA positive策略有个比较大的问题就是，会丢掉很多不带有polyA尾巴的RNA序列，比如tRNA，比如有些lncRNA以及一些特殊的mRNA。同时，我们知道细胞中含量占绝大多数的是序列重复的rRNA，如果我们把rRNA去除之后也就可以顺利进行建库测序。这个过程是对rRNA进行若干次的消化，使得rRNA都被降解而其他类型的RNA不受影响。随后再进行随机打断，反转录，加adapter。这种RNA-Seq的建库策略为rRNA - 或称为 rRNA minus。这种策略的核心是尽可能多的去除rRNA。

###  提问环节
今天的提问，偏生物细节，不过也都是我们必须要掌握的内容。

<font color="#00fff">问题1：请查阅资料，给出正常人类细胞中rRNA，tRNA，mRNA的大致比例；</font>

![](1.png)
一个典型人类细胞的RNA含量

![](2.png)
一个典型哺乳动物细胞中RNA的分布

<font color="#00fff">问题2：是不是所有人类的protein coding gene的mRNA都带有polyA尾巴？如果不是请举例；</font>

并不是，组蛋白的mRNA就没有polyA尾巴。polyA对mRNA起到稳定作用 即使其不被降解.组蛋白mRNA3'端是一茎环结构 所以组蛋白mRNA降解机制也与其他mRNA不同.

<font color="#00fff">问题3：如果一个实验想要探索与可变剪切相关的内容，应该选择polyA +的建库策略还是选择rRNA - 的建库策略？为什么？</font>

采取第一个polyA+建库策略。因为研究可变剪接的话，最后得到的都是成熟的mRNA.那么它都具有polyA,在这种情况下利用带T的磁珠进行特异性富集，将mRNA进行富集。然后建库就可以。

<font color="#00fff">问题4：rRNA minus与polyA positive这两种建库策略，哪种最终比对到参考基因组的比例会高一些？为什么？（提示，可以从建库组成，已知参考基因组的组成等方面思考）</font>

rRNA minus只是去掉了rRNA,其他的RNA没有去掉，比如说mRNA,lncRNA等等。在mapping回基因组的时候，reads的数量会多。从已知的参考基因组上来看，虽然hg38和hg19差别不大，hg38的mapping结果估计会比hg19高一些。

