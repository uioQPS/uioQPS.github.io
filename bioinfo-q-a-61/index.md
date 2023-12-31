# 【bioinfo】-Q&A-61

## 利用samtools mpileup和bcftools进行SNP calling
```
# 我们现在有bwa.bam和bow.bam两个文件。

# Pileup的输出。wgsim模拟器生成的低质量reads会被samtools忽略。

# 用samtools对fasta文件进行索引。

samtools faidx ~/refs/852/NC.fa

# 用samtools查询fasta文件。

samtools faidx ~/refs/852/NC.fa NC_002549:1-10

# 你可以罗列多个区间。

samtools faidx ~/refs/852/NC.fa NC_002549:1-10 NC_002549:100-110

# 探讨一下有参或者无参的输出。

# 查看帮助文档

samtools mpileup

# 不加参考序列的pileup使用

#* -q最低的mapping质量，默认设置是0

#* -Q最低的碱基质量，默认设置是13

samtools mpileup -Q 0 bwa.bam | more

# 有参考序列的pileup使用

samtools mpileup -f ~/refs/852/NC.fa -Q 0 bwa.bam | more

```

Pileups以及覆盖度
```
# 首先安装bcftools来处理变异call（实在不知道怎么翻译合适）出来后的格式。

#* 一般把将SNP找出来并以一定格式存储起来的过程称为SNP calling

cd ~/src

curl -OL http://sourceforge.net/projects/samtools/files/samtools/1.1/bcftools-1.1.tar.bz2

tar jxvf bcftools-1.1.tar.bz2

cd bcftools-1.1

make

ln -s ~/src/bcftools-1.1/bcftools ~/bin

# 留意下bcftools和samtools用法方面的相似性。

bcftools

# 使用Lecture8中的比对脚本。

bash compare.sh

# 每次运行完的平均覆盖度是多少？

# NR是指可能与基因组大小不相等的数目或记录。

# 默认情况下，samtools depth只输出覆盖度不为0的区域。

samtools depth bwa.bam | awk '{ sum += $3 } END { print sum/NR } '

# 如何获知基因组的大小？

# 有很多方法，但没有一个特别好的。

# 表头@SQ包含序列长度。

samtools view -h bwa.bam | head | grep @SQ

# 现在我们知道了染色体的大小了。

samtools depth bwa.bam | awk '{ sum += $3 } END { print sum/18959 } '

# 有参考序列或没参考序列的Pileups使用

# 查看使用方法

samtools mpileup

# 不加参考序列的pileup使用

samtools mpileup -Q 0 bwa.bam | more

# 有参考序列的pileup使用

samtools mpileup -f ~/refs/852/NC.fa -Q 0 bwa.bam | more

# Samtools的文本式的基因组浏览器。

# 按下?获取帮助，q或者ESC来退出。

samtools tview bwa.bam

# 为全基因组生成一个变异call出来后的存储格式文件。

samtools mpileup -Q 0 -f ~/refs/852/NC.fa -uv bwa.bam | more

# 生成基因型，然后call变异。

# Samtools是为人类基因组设计的，似乎对病毒基因组支持的不是很完美。

samtools mpileup -Q 0 -ugf ~/refs/852/NC.fa bwa.bam | bcftools call -vm > snps.vcf

# 欢迎来到你的下一个文件格式。

samtools mpileup -Q 0 -f ~/refs/852/NC.fa -uv bwa.bam | grep -v "##" | cut -f 1,2,3,4,5 | head

升级版的脚本

# 对比两个比对软件的输出。
# 使用方法: bash compare.sh
#在出错的地方停止运行。
set -ue

# 数据文件名。
READ1=r1.fq
READ2=r2.fq

#  参考序列文件。两个比对软件需要分别对它进行索引。
REFS=~/refs/852/NC.fa

# 从基因组中生成模拟reads。
# 编辑错误率并重新运行这个pipeline。
wgsim -N 10000 -e 0.1 $REFS $READ1 $READ2 > mutations.txt
# 将mutations文件转成GFF文件。

cat mutations.txt | awk -F '\t' ' BEGIN { OFS="\t"; print "##gff-version 2" } { print $1, "wgsim", "mutation", $2, $2, ".", "+",  ".", "name " $3 "/" $4 }' > mutations.gff

# 我们需要添加一个read组到mapping中。
GROUP='@RG\tID:123\tSM:liver\tLB:library1'

# 运行bwa并创建bwa.sam文件。
bwa mem -R $GROUP $REFS $READ1 $READ2 > bwa.sam

# 运行bowtie2并创建bow.sam文件。
bowtie2 -x $REFS -1 $READ1 -2 $READ2 > bow.sam

# 调整bowtie2
# bowtie2 -D 20 -R 3 -N 1 -L 20 -i S,1,0.50 -x $REFS -1 $READ1 -2 $READ2 > bow.sam

# 对每个sam文件，都转换成bam（sam的二进制）格式。
for name in *.sam;
do
samtools view -Sb $name > tmp.bam
samtools sort -f tmp.bam $name.bam
done

# 删除中间文件。
rm -f tmp.sam tmp.bam

# 修改名字，使得他们不再含有两个后缀。
mv bwa.sam.bam bwa.bam
mv bow.sam.bam bow.bam

# 对数据进行索引。
for name in *.bamdosamtools index $namedone

# 删除这个程序生成的所有文件。
# rm -f *.bam *.bai *fq *.txt *.sam *.gff
```

