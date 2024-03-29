# 【bioinfo】-Q&A-「4」

## Illumina测序技术细节探究 II
我们接着第2个问题，第3个问题接着问。既然我们已经知道的Illumina测序的基本概念，基本过程；也知道了Illumina测序的主要特点。那么还是要有个细节的探索。

---
1. Illumina目前主流的测序仪都有哪几种型号？各自大概的通量是多少？（也就是1个run能跑出多少数据）

目前主流的测序仪及其通量主要是Hiseq2500（50-1000Gb）、Hiseq3000（125-750Gb）、Hiseq4000（125-1500Gb）、Hiseq X Five（900-1800Gb）和Hiseq X Ten（900-1800Gb）

---

2. Illumina目前的测序技术，最核心的就是边合成边测序，即我们常说的 Sequencing by synthesis （SBS），那么为什么能够实现SBS？

经过桥式PCR之后同一段序列已经成簇，下一段就是开始进行测序，这一步比较简单，就是加入primer，然后添加经过特殊处理的ATCG四种碱基，特殊的地方有两点：一个是碱基部分加入了荧光基团，可以激发出不同的颜色，另一个是脱氧核糖3号位加入了叠氮基团而不是常规的羟基，这个叠氮集团保证了每次只能够在序列上添加1个碱基.

这样每1轮测序，保证只有1个碱基加入的当前测序链。这时候测序仪会发出激发光，并扫描荧光。因为一个cluster中所有的序列是一样的，所以理论上，这时候cluster中发出的荧光应该颜色一致。随后加入试剂，将脱氧核糖3号位的—N2改变成—OH，然后切掉部分荧光基团，使其在下一轮反应中，不再发出荧光。如此往复，就可以测出序列的内容。
![](../../../../../Desktop/md/【bioinfo】-Q-A-「4」/du.jpg)

---

3. 我们在第1问中，问了大家一个问题“Illumina测序技术为什么不能像第1代测序技术一样测500bp以上？”，这里面主要涉及到两种错误，一种叫phasing，一种叫pre-phasing，分别是什么意思？

通俗来讲phasing表示本来同步添加的碱基有一些没加上，而pre-phasing则是加多了，都会导致当前bp的荧光检测出现噪音，造成phasing的主要原因是合成酶的活性降低，而pre-phasing则可能是叠氮基团性质不稳定，转化为羟基在一步检测中添加了不止一个碱基。

**reference:** [Diagnosing problems with phasing and pre-phasing on Illumina platforms](http://lab.loman.net/high-throughput%20sequencing/2013/11/21/diagnosing-problems-with-phasing-and-pre-phasing-on-illumina-platforms/)

---

