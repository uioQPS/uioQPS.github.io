# 【bioinfo】-Q&A-「2」

## 测序技术初探
现在我们实验室或者公司常用第1代测序与第2代测序，那么：

---
1. 第1代测序 sanger 测序法的原理是什么？通量比较低的核心原因是什么？

sanger法测序及双脱氧链终止法，它采取DNA复制原理，通过在DNA复制过程中添加双脱氧三磷酸核苷酸（ddNTP）终止DNA链的延伸，在DNA链不同位置的延伸终止判断该位置的碱基类型。但是凝胶电泳的时间较长，导致sanger法测序通量低。

---
2. 作为2006年正式发布的illumina测序技术，或者称为第2代测序技术的代表性技术，其最大的特点是什么？

高通量，成本低，但测序长度较短。

___

3. Illumina测序技术的核心是什么？

核心内容有两个，一个是桥式PCR，主要用于扩大信号；另一个是4色荧光可逆终止反应，使illumina测序可以实现边合成边测序的技术。

___

4. Illumina测序技术为什么不能像第1代测序技术一样测500bp以上？

主要的原因有两个，一方面测序时，经过长时间的PCR，会有不同步的情况。比如一开始1个cluster中是100个完全一样的DNA链，但是经过1轮增加碱基，其中99个都加入了1个碱基，显示了红色，另外1个没有加入碱基，不显示颜色。这时候整体为红色，我们可以顺利得到结果。随后，在第2轮再加入碱基进行合成的时候，之前没有加入的加入了1个碱基显示红色，剩下的99个显示绿色，这个时候就会出现杂信号。当测序长度不断延长，这个杂信号会越来越多，最后很有可能出现50个红，50个绿色，这时信号不足以判断碱基类型；第二就是测序过程中合成酶的活性越来越不稳定，后面碱基添加出现问题。

___

