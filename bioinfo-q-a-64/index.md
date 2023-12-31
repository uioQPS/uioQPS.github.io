# 【bioinfo】-Q&A-64

## 新冠病毒是源自实验室的？

在各种各样的质疑新冠病毒可能源自实验室的文章，水准最高的是美国纯净和应用知识研究所（Institute for Pure and Applied Knowledge, IPAK）的建立者和CEO，Dr. James Lyons-Weiler在网上贴的两篇英文的技术贴，认为2019-nCoV有可能来源于实验室。这些菌株在实验室里连接了通用载体pShuttle-SN，而连接这个载体制备的菌株，主要是为了生产冠状病毒的疫苗。原文链接就不放了，两个帖子的主要内容是，Dr. James Lyons-Weiler将2019-nCoV和其他冠状病毒的基因组序列进行比对，发现2019-nCoV存在一段在其他冠毒里不存在的、长度为1,378 bp的插入片段（INS1378），将这段序列与pShuttle-SN重组载体序列进行比对，有67%的同一性（下图）。

![](1png)



因此，Dr. James Lyons-Weiler的观点是：新冠病毒来自实验室的重组病毒，该*重组组病毒主要是为了生产疫苗（A recombined virus made in a laboratory for the purpose of creating a vaccine）*。事实上，James并没有实锤他的观点是不是对的，因此他第二篇帖子的题目用了“中等强度的确认”（Moderately Strong Confirmation）。老外写东西，一般正方反方的观点都会列举，他自己总结的支持或反对这个论点的证据如下：

A. 支持的证据：INS1378与pShuttle-SN之间的相似性；存在蝙蝠冠毒中的类SARS的Spike蛋白，或者与冠毒非常相似；自展值低。

B. 反对的证据：低序列相似性（指INS1378与pShuttle-SN之间，但仍然高度显著）；这些RNA病毒可能快速演化（例如在实验室条件下）。

C. 观点的可能性：高度可能。

D. 验证：检测中国实验室里的所有在研的冠毒（一例即可证明）；在野外分离一个菌株能够与2019-nCoV匹配（一例即可证伪）。



注意Dr. James Lyons-Weiler在（D）里描述的验证方法。事实上要实锤James的观点是正确的，比较困难，因为必须要检测到一株实验室里在研的、用来生产疫苗、并且与2019-nCoV高度相似的病毒菌株。如果找不到这样的菌株，既不能证明James的观点是对，也不能证明他的观点一定是错的。但是James也提出了证伪的方法，那就是找到一株在野外分离的菌株能够与2019-nCoV匹配，这样即可表明他的观点是错的。

================================================

这里我们先讲结论：James的计算分析有误，其结果不能支持新冠病毒源自实验室的猜想。下面我们开始用“简单的生物信息学分析”进行证明：

#### 第一，我们需要先知道冠状病毒的突变速率。
查阅文献，2004年Zhao等人发表论文，推测SARS-CoV基因组的突变速率约是0.80 - 2.38 x 10^-3个碱基/位点/年，这个结果与其他RNA病毒有相同的数量级（https://www.ncbi.nlm.nih.gov/pubmed/15222897）。我们考虑上限，一个SARS-CoV基因组的突变率应当不超过3 x 10^-3个碱基/位点/年，2019-nCoV的基因组长度为29,903 bp（https://www.ncbi.nlm.nih.gov/nuccore/NC_045512），这样可以算出来2019-nCoV每年突变的碱基应该<29,903*3x 10^-3=~90 bp。由于计算方法不同，估算出来的突变速率可能不尽相同；病毒基因组的各个部分，可能进化速率也不一致。例如，2004年一篇Science上估算SARS-CoV的突变率为8.26 × 10^–6个碱基/位点/天（https://www.ncbi.nlm.nih.gov/pubmed/14752165），换算得到2019-nCoV每年突变碱基为90 bp（8.26 × 10^–6*365*29,903），与我上面的分析一致。2004年另一篇论文估算SARS-CoV的突变率为5.7 × 10^–6个碱基/位点/天（https://www.ncbi.nlm.nih.gov/pubmed/15347429），换算之后得到2019-nCoV每年突变碱基为62 bp。另外，需要注意的是，目前的分析表明，2019-nCoV的进化速率比较低，有研究者估算2019-nCoV的突变率是2.067×10^-4个碱基/位点/年（http://virological.org/t/temporal-signal-and-the-evolutionary-rate-of-2019-n-cov-using-47-genomes-collected-by-feb-01-2020/379），换算之后大约是每年全基因组6个碱基突变，远比我假设的突变率要低。另外，也有学者使用不同的模型，估算2019-nCoV的突变速率约为0.42 x 10^-3 – 1.89 x 10^-3，这个数字也比我使用的要小。因此，我们估算2019-nCoV每年大约突变90个碱基位点，这个数字可以认为是上限。

#### 第二，中科院病毒所石正丽研究员等人于1月23日在线提交了一篇预印本文章
标题为《Discovery of a novel coronavirus associated with the recent pneumonia outbreak in humans and its potential bat origin》（https://www.biorxiv.org/content/10.1101/2020.01.22.914952v2），其中很重要的一个分析就是发现2019-nCoV与从云南省的某种菊头蝠（Rhinolophus affinis）里分离得到的BatCoV RaTG13（https://www.ncbi.nlm.nih.gov/nuccore/MN996532.1，基因组长度29,855 bp）有96.2%的全基因组相似性。我们把2019-nCoV（核酸序列标识符NC_045512）与BatCoV RaTG13（核酸序列标识符MN996532）做双序列比对，在NCBI BLAST（https://blast.ncbi.nlm.nih.gov/Blast.cgi）点击“Global Align”，进入到双序列比对的页面.




两者同一性96%，有29,905-28,717=1,188对碱基差异，由于两者可以独立演化，全基因组突变速率3 x 10^-3个碱基/位点/年，2019-nCoV和BatCoV RaTG13每年大约分别全基因突变90 bp，因此大致可以估算两者的分歧时间大约是：1,188/(90*2)=6.6年。在石老师的文章里，他们先利用较短的RNA依赖的RNA聚合酶（RdRp）区域确定与2019-nCoV可能相似的蝙蝠来源的冠毒，根据结果对BatCoV RaTG13进行了全基因组测序。因此，第一，BatCoV RaTG13可能是实验室里目前找到的、与2019-nCoV最相似的病毒；第二，如果石老师的实验室发生泄漏，BatCoV RaTG13在自然界中独立演化成2019-nCoV，至少需要6.6年。第三，由于2019-nCoV需要演化6.6年，那么必须在野外找到大量的过渡性、相似度与2019-nCoV更高的菌株，才能证明实验室泄露是可能发生的。

#### 第三，James宣称的INS1378是2019-nCoV独有的片段，这个分析是不对的。
我们根据James提供的序列（https://jameslyonsweiler.com/wp-content/uploads/2020/02/inserted-portion.txt），在https://blast.ncbi.nlm.nih.gov/Blast.cgi页面，点击“Nucleotide BLAST”选项，进入页面之后输入这段序列，“Program Selection”里选择“Somewhat similar sequences (blastn)”进行核酸序列的比对，“Algorithm parameters”的Max target sequences选择“250”，这样能看到更多的比对结果。然后点击BLAST找相似性序列，部分结果如下：

![](2.png)



注意从下往上数的第3和第4个结果，分别是蝙蝠来源的类SARS-CoV，包括bat-SL-CoVZC45和bat-SL-CoVZXC21。我们点击“Bat SARS-like coronavirus isolate bat-SL-CoVZC45, complete genome”，部分比对结果如下：

![](3.png)



可以发现INS1378在bat-SL-CoVZC45里有相似度较高的片段。在刚才的比对结果页面，使劲的往下翻，任意点击一个比方说SARS coronavirus PC4-13, complete genome，


因此INS1378在SARS-CoV PC4-13里也有相似性片段。所以结论是INS1378在冠状病毒里广泛存在相似片段，并不是仅在2019-nCoV或BatCoV RaTG13里独有。

#### 第四，INS1378有没有可能来源于pShuttle-SN载体？
根据James提供的pShuttle-SN载体的序列标识符（https://www.ncbi.nlm.nih.gov/nuccore/AY862402，长度5,607 bp），我们用BLAST页面的“Global Align”做全局双序列比对



两者的整体相似性是20%，这么低的相似程度，不能说明INS1378来源于pShuttle-SN载体。由于BLAST的“Global Align”用的是Needleman-Wunsch算法做全局双序列比对，我们换个工具，用基于Smith-Waterman算法的EMBOSS Water（https://www.ebi.ac.uk/Tools/psa/emboss_water/）来做局部双序列比对.



可以看到这两条序列之间有局部的66.0%的同一性，差异的碱基对是1387-916=471 bp。我们上面讲过了，冠毒全基因组突变速率不超过3 x 10^-3个碱基/位点/年，因此长度为1387 bp的片段，每年平均突变4个位点。实验室用的载体，是恒定的、不发生自然演化的，因此INS1378如果来源于pShuttle-SN载体，其独立进化的时间是471/4=~118年。James在博文中讲了pShuttle-SN载体是Bert Vogelstein在1998年构建的，距今22年，因此INS1378不可能从pShuttle-SN载体演化出来。另外，所谓的载体，应当是全长才会有效，没有证据表明pShuttle-SN载体中长度约为1378 bp的片段能够单独发挥作用。

#### Conclusion
第一，如果2019-nCoV源自实验室泄露的BatCoV RaTG13，这个泄露事件应当至少在6.6年之前，且野外必须能够找到大量的、介于2019-nCoV和BatCoV RaTG13之间，且与2019-nCoV具有更高相似度的病毒菌株。

第二，James宣称的仅在2019-nCoV里存在的长度为1387 bp的片段，即INS1378，事实上在各种冠毒中广泛存在相似性片段。

第三，INS1378与pShuttle-SN载体的序列全局相似性只有20%，局部相似性也并不高，不能支持INS1378源自pShuttle-SN载体的观点。第四，James讲的那个“自展值低”是怎么回事？主要是因为目前测序的2019-nCoV的基因组序列之间相似度太高，突变太少，所以自展检验估的不准。

