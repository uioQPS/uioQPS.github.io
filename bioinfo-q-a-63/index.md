# 【bioinfo】-Q&A-63

## Python计算Rank(秩)
在统计学中，有时候我们需要求一组数据的Rank，如非参检验、Spearman相关性等。Rank中文意思叫做顺序，更专业点的叫法是”秩”。这里介绍一下用Python求Rank。
先看一个简单的例子：
```
>>> a = [1.0, 5.6, 3.5, 9.7, 7.8]
>>> # rank_a = [1, 3, 2, 5, 4]
...
>>> rank_a = sorted(range(len(a)), key = a.__getitem__)
>>> rank_a
[0, 2, 1, 4, 3]
>>> rank_a = [i + 1 for i in rank_a]
>>> rank_a
[1, 3, 2, 5, 4]
```
上面的例子中，列表a中的元素是没有重复的，直接排序即可。如果有重复元素怎么办呢？最简单的办法是使用SciPy提供的rankdata()函数。
```
>>> from scipy.stats import rankdata
>>> rankdata([0, 2, 3, 2])
array([ 1. ,  2.5,  4. ,  2.5])
>>> rankdata([0, 2, 3, 2], method='min')
array([ 1,  2,  4,  2])
>>> rankdata([0, 2, 3, 2], method='max')
array([ 1,  3,  4,  3])
>>> rankdata([0, 2, 3, 2], method='dense')
array([ 1,  2,  3,  2])
>>> rankdata([0, 2, 3, 2], method='ordinal')
array([ 1,  2,  4,  3])
```
rankdata()函数返回一个array对象，默认的方法是average，即相同元素的Rank值取算术平均数，同时rankdata()函数还提供了其他方法。
如果觉得安装SciPy库太麻烦，我们也可以自己实现。下面的代码计算重复元素的Rank使用的方法是average，返回一个列表。
```python
def rank_simple(vector):
    return sorted(range(len(vector)), key=vector.__getitem__)
def rankdata(vector):
    n = len(vector)
    ivec = rank_simple(vector)
    svec = [vector[rank] for rank in ivec]
    sumranks = 0
    dupcount = 0
    newvector = [0] * n
    for i in range(n):
        sumranks = sumranks + i
        dupcount = dupcount + 1
        if i == n - 1 or svec[i] != svec[i + 1]:
            averank = sumranks / float(dupcount) + 1
            for j in range(i - dupcount + 1, i + 1):
                newvector[ivec[j]] = averank
            sumranks = 0
            dupcount = 0
```

