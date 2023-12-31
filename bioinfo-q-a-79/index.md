# 【bioinfo】-Q&A-79

## Hi-C III

#### 从 .hic 文件提取数据
此功能由 Juicebox tools 的 dump 命令完成。
https://github.com/aidenlab/juicer/wiki/Data-Extraction

#### Arrowhead
finding contact domains.
```
arrowhead <HiC file> <output_file>
https://github.com/aidenlab/juicer/wiki/Arrowhead
```
生成一个12列的矩阵，每行包含以下信息

chromosome1	x1	x2	chromosome2	y1	y2	color	corner_score	Uvar	Lvar	Usign	Lsign
```
Explanations of each field are as follows:
chromosome = the chromosome that the domain is located on
x1,x2/y1,y2 = the interval spanned by the domain (contact domains manifest as squares on the diagonal of a Hi-C matrix and as such: x1=y1, x2=y2)
color = the color that the feature will be rendered as if loaded in Juicebox
corner_score = the corner score, a score indicating the likelihood that a pixel is at the corner of a contact domain. Higher values indicate a greater likelihood of being at the corner of a domain
Uvar = the variance of the upper triangle
Lvar = the variance of the lower triangle
Usign = -1*(sum of the sign of the entries in the upper triangle)
Lsign = sum of the sign of the entries in the lower triangle
```

#### HiCCUPS
HiCCUPS is an algorithm for finding chromatin loops.
https://github.com/aidenlab/juicer/wiki/HiCCUPS
分为两个版本，gpu版本必须安装对应版本的CUDA。如果不能调用gpu的，可以使用cpu版本，加上参数--cpu。

与gpu版本的区别是限制在对角线8M范围内进行搜索，所以会有遗漏，但是大部分都会找到（The vast majority of peaks (98%) reflected loops between loci that are <2 Mb apart）。
```
hiccups [-m matrixSize] [-c chromosome(s)] [-r resolution(s)]
        <HiC file> <outputDirectory>
```
#### HiCCUPSDiff
HiCCUPS Diff allows you to find differential loops between loop lists.
https://github.com/aidenlab/juicer/wiki/HiCCUPSDiff

#### APA

