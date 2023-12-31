# 【bioinfo】-Q&A-80

## methy-ATAC-seq同时测甲基化和染色质的可及性
伴随着10X genomics最近推出的sc-ATACseq的protocol，ATAC-seq技术将会掀起一股新的研究热潮。尤其是ATAC-seq与其他技术的结合，再搭上单细胞的快车，很多科学问题借助这个新的研究方法也许会有新的发现，至少会涌现出一批paper。
恰好看到群友推荐这篇methy-ATAC-seq的文章，可以同时检测甲基化与accessible chromatin，虽然目前还不是单细胞水平，不过还是值得一看。

>doi: http://dx.doi.org/10.1101/445486

#### 问题的提出
分开检测染色质的可及性和甲基化然后再整合分析，不能准确保证数据的一致性。将两种方法整合在一起，一次性检测染色质的可及性以及该区域的甲基化。

#### mATAC-seq方法
- 甲基化寡核苷酸被加载到Tn5上产生新的转座子
- 片段化
- 甲基脱氧胞嘧啶三磷酸（5-MdCTP）在转位后的末端修复反应中取代DCTP

这种修饰在mATAC-seq文库制备的最后步骤中保护Nextera adapter序列，这些序列是经过Tn5处理的染色质。

![](1.webp)

#### 验证mATAC-seq与Omni-ATAC-seq和WGBS的一致性
- DNA methylation detected by mATAC-seq replicates was highly reproducible at CpG Islands (r 2 =0.95) and peaks (r 2 =0.83)
- methylation levels reported by mATAC-seq correlated well with levels reported by WGBS at CpG Islands (r 2 =0.86) and peaks (r 2 =0.68)

![](2.webp)

#### peak处的可及性和甲基化
- 显著性peak区域的RPKM和甲基化变化，以及相关的motif
- promotor区域的甲基化变化和可及性的比较
- mRNA的变化与promotor区域的可及性变化
![](3.webp)

#### 可及性和甲基化的整合分析
- 聚类
- 模块内启动子处的基因表达变化
- RPKM,methylation,H2A.Z,启动子、增强子、CTCF的谱图变化
- 模块内的motif
![](4.webp)

