# 【bioinfo】-Q&A-「3」

## Illumina测序技术细节探究
目前我们最常使用的就是Illumina公司的测序技术，Illumina公司的测序技术最明显的几个特点是：价格低，通量高，测序读长短。那么我们今天的问题，就是围绕Illumina测序技术的细节来提问的。

---
1. 什么是Illumina测序adapter？同一批上机的adapter序列一样吗？它的作用是什么？

adapter的中文意思为适配器或者接口，在illumina测序过程中关键一步是将文库片段固定在flowcell上，然后通过桥式PCR将片段扩增，在被打断成300~500bp的长度的片段末端被补平后adaptor将被添加到片段两端，一方面用于将片段固定在flowcell上，同时adaptor中还包含桥式PCR所需要的引物
![](../../../../../Desktop/md/【bioinfo】-Q-A-「3」/x10.jpg)

---
2. 一个完整的Illumina测序过程是那几步？

完整的测序过程仅包含两步，第一是桥式PCR扩增，第二是以4色荧光可逆终止反应为核心技术的测序；

---

3. 什么是桥式PCR技术？为什么要进行桥式PCR？
加上adaptor之后的DNA样品与flowcell上固定的oligo（寡链核苷酸）匹配后就被固定在flowcell上，通过桥式PCR进行扩增成cluster,便于后面的荧光测序，主要步骤为：
![](../../../../../Desktop/md/【bioinfo】-Q-A-「3」/flow.jpg)

进行第一轮扩增，将序列补成双链。加入NaOH强碱性溶液破坏DNA的双链，并洗脱。由于最开始的序列是使用化学键连接的，所以不会被洗。
加入缓冲溶液，这时候序列自由端的部分就会和旁边的oligo进行匹配
进行一轮PCR，在PCR的过程中，序列是弯成桥状，所以叫桥式PCR，一轮桥式PCR可以使得序列扩增1倍
如此循环下去，就会得到一个具有完全相同序列的cluster

---

4. 我们都说，测序结果会包含index，那么index是什么？有什么作用？

一条lane能测得的数据量在30G左右，而一个样品的测序量一般不会这么大，所以在建库的时候对每一种样品的接头加上不同的标签序列，这个标签就叫做Index，有了index就可以同时在一个lane中测多种数据了，后期可以根据index将数据分开；
![](../../../../../Desktop/md/【bioinfo】-Q-A-「3」/seq.png)

---

5. 我们所说的flowcell，lane，tile都是什么意思？

flowcell 是指Illumina测序时，测序反应发生的位置，1个flowcell含有8条lane
lane 每一个flowcell上都有8条泳道，用于测序反应，可以添加试剂，洗脱等等
tile 每一次测序荧光扫描的最小单位
![](../../../../../Desktop/md/【bioinfo】-Q-A-「3」/lane.jpg)

---
6. Illumina测序结果质量表示方法采用的是Phred33还是Phred64？
最新的测序质量结果一般都为Phred33，但是早期的测序数据可能出现Phred64。

---

