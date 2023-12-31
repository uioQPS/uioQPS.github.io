# 【bioinfo】-Q&A-77

## Hi-C I
![](1.webp)


```python
#创建工作目录，其后所有操作都在此目录下进行
mkdir ./opt
cd opt
#克隆 juicer
git clone https://github.com/theaidenlab/juicer.git
ln -s juicer/CPU scripts
cd scripts/common
wget http://hicfiles.tc4ga.com.s3.amazonaws.com/public/juicer/juicer_tools.1.7.6_jcuda.0.8.jar
ln -s juicer_tools.1.7.6_jcuda.0.8.jar juicer_tools.jar
cd ../..
# 参考基因组建立索引，我用的是 Homo_sapiens_assembly38.fasta
mkdir references
cd references
cp <path>/Homo_sapiens_assembly38.fasta  # 或者 ln -s <path>/Homo_sapiens_assembly38.fasta
bwa index  Homo_sapiens_assembly38.fasta
cd ..
# 添加限制性内切酶位点信息，注意自己的是不是 MboI 酶。
mkdir restriction_sites
cd restriction_sites/
python2 ../juicer/misc/generate_site_positions.py  MboI  hg38_MboI ../references/Homo_sapiens_assembly38.fasta # 生成了 hg38_MboI.txt 文件
awk 'BEGIN{OFS="\t"}{print $1, $NF}' hg38_MboI.txt > hg38.chrom.sizes
cd ..
# 添加 fastq 文件 (官方测试文件)
mkdir fastq && cd fastq
nohub wget http://juicerawsmirror.s3.amazonaws.com/opt/juicer/work/HIC003/fastq/HIC003_S2_L001_R1_001.fastq.gz &
nohub wget http://juicerawsmirror.s3.amazonaws.com/opt/juicer/work/HIC003/fastq/HIC003_S2_L001_R2_001.fastq.gz &
cd ..
# 运行 Juicer
bash scripts/juicer.sh  -d <path/opt>  -D <path/opt> -y restriction_sites/hg38_MboI.txt  -z references/Homo_sapiens_assembly38.fasta -p restriction_sites/hg38.chrom.sizes -s MboI
# 上面-z -p 设置绝对路径吧，要不然会识别不了。我直接改软件源码了，-d -D 都设置了还需要绝对路径，作者写的不人性化啊
```
fastq等文件输入 ---》bwa比对 ---》排序 ---》合并 ---》去PCR重复 ---》生成 hic文件

#### 结果文件

结果文件都放在了生成的 aligned 中，主要文件是 .hic 文件，其中的inter_30.hic 是设置了 mapq threshold >30 后得到的结果。如果 GPU （cudd）可用，软件还会自动使用自带 HiCCUPS 算法计算 contact domains 并保存在 inter_30_contact_domains 文件夹中。如果GPU不可用，可手动用自带的 CPU HiCCUPS 算法来计算，后面章节有介绍。

hic文件格式请看 https://github.com/theaidenlab/juicebox/blob/master/HiC_format_v8.docx

#### 重复样本合并

```
while getopts "d:g:hxs:SD:" opt; do
    case $opt in
        g) genomeID=$OPTARG ;;
        h) printHelpAndExit 0;;
        d) topDir=$OPTARG ;;
        s) site=$OPTARG ;;
        x) exclude=1 ;;
        S) stage=$OPTARG ;;
        D) juiceDir=$OPTARG ;;
        [?]) printHelpAndExit 1;;
    esac
done
```

