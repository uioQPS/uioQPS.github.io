# 【bioinfo】-Q&A-62

## T检验靠谱吗？

对于实验生物学研究者来说，经常遇到的一个场景是，对照组测量3个值，实验组测量3个值，然后用T检验得出均值是否有显著性差异，然后做出结论。如下图所示：

bio_t

那么T检验靠谱吗？本文采用模拟抽样的方法来探讨这件事情，由于本人统计学知识有限，还请各位读者批评指正。

我们通过构建两个随机总体，总体1的均值固定为1（模拟control），总体2的均值我们从依次取0.5到2.0（模拟treatment相对与control的变化），标准差我们取0.01、0.05、0.10（模拟测量误差1%，5%，10%）。总体数量为1000，每次随机抽取3个样本，抽样1000次，考察通过T检验得出正确结论的频率。代码如下：
``` python
import numpy as np
import pandas as pd
import seaborn as sns
from scipy import stats
import matplotlib.pyplot as plt
plt.xkcd()
sns.set(style="white", context="talk")
def norm_t(loc1, loc2, scale1, scale2, n):
    normSpace1 = stats.norm.rvs(loc = loc1, scale = scale1, size = 1000)
    normSpace2 = stats.norm.rvs(loc = loc2, scale = scale2, size = 1000)
    count = 0
    for i in range(1000):
        sample1 = np.random.choice(normSpace1, n)
        sample2 = np.random.choice(normSpace2, n)
        t, p = stats.ttest_ind(sample1, sample2)
        if (p &lt; 0.05 and (sample1.mean() &gt; sample2.mean()) == (loc1 &gt; loc2)):
            count = count + 1
    return count / 1000
def plot(stds):
    df = pd.DataFrame()
    for std in stds:
        x = np.arange(0.5, 2.1, 0.1)
        y = [norm_t(loc1 = 1, loc2 = i, scale1 = std, scale2 = std, n = 3) for i in x]
        z = [std for i in x]
        df_std = pd.DataFrame([x, y, z]).T
        df_std.columns = ["difference", "sensitivity", "std"]
        if df.shape == (0, 0):
            df = df_std
        else:
            df = df.append(df_std)
    sns.pointplot(x = "difference", y = "sensitivity", hue = "std", data = df)
plot([0.01, 0.05, 0.10])
```
![](1.png)
std_sensitivity

通过上图，我们发现，当标准差我们取0.01（模拟测量误差1%）时，增大到1.1倍或者减小到0.9倍，正确率可以达到100%。而当标准差取0.05和0.10（模拟测量误差5%和10%），效果就不理想了，本来是有差异的，还是有很大几率通过T检验得出错误的结论。显而易见的是，随着**标准差的增加，T检验的效果变差了。**

那么当标准差取0.10时，随着**抽样样本数**的增加，T检验判断正确的频率会是怎么样的呢？请看如下代码：
```python
def plot2(diffs):
    df = pd.DataFrame()
    for diff in diffs:
        x = list(range(3, 60, 3))
        y = [norm_t(loc1 = 1, loc2 =1.1, scale1 = 0.10, scale2 = 0.10, n = i) for i in x]
        z = [diff for i in x]
        df_diff = pd.DataFrame([x, y, z]).T
        df_diff.columns = ["samle number", "sensitivity", "difference"]
        if df.shape == (0, 0):
            df = df_diff
        else:
            df = df.append(df_diff)
    sns.pointplot(x = "samle number", y = "sensitivity", hue = "difference", data = df)
plot2([0.5, 0.8, 1.2, 2.0])
```
![](2.png)
n_sensitivity

通过上图，我们发现，即使标准差达到0.10，当样本数量达到30以上时，T检验的效果就非常好了。     

那么如果两个总体没有差异，T检验得出有差异的错误结论的情况是什么样子的呢？我们构建两个均值为1容量为1000的总体，每次随机抽取3个样本，在**不同标准差及样本数目的**情况下，考察T检验得出正确结论的频率。请看代码：
```python
def norm_t2(loc, scale1, scale2, n):
    normSpace1 = stats.norm.rvs(loc = loc, scale = scale1, size = 1000)
    normSpace2 = stats.norm.rvs(loc = loc, scale = scale2, size = 1000)
    count = 0
    for i in range(1000):
        sample1 = np.random.choice(normSpace1, n)
        sample2 = np.random.choice(normSpace2, n)
        t, p = stats.ttest_ind(sample1, sample2)
        if p &gt; 0.05:
            count = count + 1
    return count / 1000
def plot3(stds):
    stds = [0.1, 0.5, 1]
    df = pd.DataFrame()
    for std in stds:
        x = list(range(3, 60, 3))
        y = [norm_t2(loc = 1, scale1 = std , scale2 = std, n = i) for i in x]
        z = [std for i in x]
        df_std = pd.DataFrame([x, y, z]).T
        df_std.columns = ["samle number", "specificity", "std"]
        if df.shape == (0, 0):
            df = df_std
        else:
            df = df.append(df_std)
    sns.pointplot(x = "samle number", y = "specificity", hue = "std", data = df)
    plt.ylim(0, 1.2)
plot3([0.1, 0.5, 1])
```
![](3.png)
specificity

通过上图，我们惊讶地发现，**当两个总体没有差异时，不管标准差是多少和取样数目是多少，T检验都有非常好的表现，即得到差异不显著的的结论。**

这里构建的总体都是正太总体，其实对于其他分布的总体，结论是一样的，这里就不展示了。
通过上面的探索，我们可以得到如下结论：
1. 对于两个未知总体，通过T检验考察均值是否有显著差异，如果得出结论是差异不显著，那么进一步分析这些数据的标准差是否太大了，考虑是否增加抽样样本数做进一步分析。
2. 对于两个未知总体，通过T检验考察均值是否有显著差异，如果得出结论是差异显著，那么请相信它吧！

如果差异不显著，我们通过增大样本数量，使得差异显著；如果差异显著，那就是一个好结果嘛！
哈哈，T检验是实验生物学家的利器啊！

