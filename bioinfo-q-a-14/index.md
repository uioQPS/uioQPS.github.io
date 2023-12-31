# 【bioinfo】-Q&A-14

## 第14题 你应该知道的BLAST
Hello,大家好！

从第13问开始，我们开启了序列比对之旅。

之前的第13题本质上是为了让大家学习双序列比对（pairwise alignment），接下来，我们不会去讲多序列比对（multiple alignment）的算法问题，因为多序列比对的不同算法各种各样，但很多时候的思路都是把多序列比对，分解成若干个双序列比对的问题，然后再进行最后的结果整合。所以，我们把pairwise alignment的原理与算法搞定了，就已经很OK了~

今天我们带大家思考1个问题，就是为什么要开发BLAST算法？

我们在第13问中学习到的是pairwise alignment的两种算法，全局比对算法与局部比对算法，无论哪种算法，得到的都是2条序列比对的最优解，当然某些时候最优解有可能有多个。那所有的序列比对问题是不是都可以用这种算法来解决了呢？ 我们来算这么1笔账。

![](../../../../../Desktop/md/【bioinfo】-Q-A-14/1.jpg)

图1 常用的搜索核酸库里面大约有47M条序列

假设我有1条序列 SeqA = 100bp（这个不是很长哟~），我想找到这条序列的相似序列。目前已知的非冗余核酸序列库，序列有47193206条，按平均长度在0.1Mbp左右。如果我们想要找到SeqA的相似序列，使用局部比对算法，那么至少就需要100bp × 0.1Mbp × 47193206条 次比较运算等于471930 × 10^9，现在服务器最快的CPU，单核心1秒可以大约运行3×10^9次，假设我们的服务器有20个核心，那么 ——

完成任务大约需要的时间 = 471930 × 10^9 ÷ 3 × 10^9 ÷ 20 = 7860秒 大约是130分钟；

如果我们的SeqA = 1000bp（这个也不是很长哟~ ）那么时间就变成了1300分钟；

我们就为了找个相似性，就要花2个小时？这个肯定不划算吧？ 所以，有了这个需求以后，一帮大佬就搞出个优化算法叫BLAST（Basic Local Alignment Search Tool）把上面这个任务的运行时间缩短到了1分钟以内。

我们今天的问题，也是请大家去观看北京大学-生物信息学导论里面的视频：

[生物信息学：导论与方法(北京大学) - BLAST](https://www.bilibili.com/video/av10042290/?p=14)

在看完视频以后，请大家回答问题：

**1. BLAST提高搜索速度的核心算法的名称是什么？**
```
启发式算法
```
​**2. BLAST结果中 P-value 与 E-value分别是什么意思？P-value可以大于1吗？E-value可以大于1吗？**
```
在BLAST结果中每一条匹配序列都会有匹配的score值和E值;
S值表示两序列的相似度，分值越高表明它们之间相似的程度越大;
E值是可靠性的评价，它表明在随机的情况下出现这种相似度的序列的条数。
--------------------
S值越大越好，最大是100%;
E值越小越好，注意E值有可能大于1!
```
**3. 如果想要降低BLAST的假阳性，你通常需要做什么？**
```
- Word size的选择，BLAST算法将⽬目标序列列分割成一系列具有字段长度的小的序列进行数据库搜索，因此当此值越小得到的搜索结果越多，假阳性也就越多。
- 根据序列长度调整E值，如果检索序列较短可适当提高E值，反之可降低E值。
- 空位罚分的选择，严谨的罚分会让相似度高的序列错过，而松弛的罚分会使检索结果过多。
- 序列检索前将低复杂度的序列先去除，尤其是DNA序列列中的重复片段。
```
**4. 假设给你一条序列，运行结果中序列相似度最高的来自于哪个物种？**
```
>Protein Sequence
MVRAPCCEKMGLKKGPWTPEEDQILISYIQSNGHG
NWRALPKLAGLLRCGKSCRLRWTNYLRPDIKRGNF
TREEEDSIIQLHEMLGNRWSAIAARLPGRTDNEIK
NVWHTHLKKRLKNYQPPQSSKRHSKNKDSKAPCTS
QIALKSSNNFSNIKEDGPGLGSGPNSPQLSSSEMS
TVTADSLAVTMDISNSNDQIDSSENFIPEIDESFW
TDGLSTSGGGEELQVQFPFHDMKQENVEKDVGAKL
EDDMDFWYSVFIKSGDLLELPEF
# 使用网站
BLAST：http://blast.ncbi.nlm.nih.gov
# 参数设置：
Database: Non-redundant protein sequences (nr)
Algorithm: blastp
Word size: 3
Matrix: BLOSUM62
Gap Costs: Existence: 11 Extension: 1
其他参数默认
```
运⾏结果:
![](../../../../../Desktop/md/【bioinfo】-Q-A-14/2.png)
由图中结果可知，序列相似度最高的物种是Solanum lycopersicum，番茄。

