##  numpy百题大冲关

### 第一部分：创建数组

**1.利用列表创建一个一维数组**

注意：`numpy.array` 和 Python 标准库 `array.array` 并不相同，前者更为强大。

```python
np.array([1, 2, 3])
```



**2.通过列表创建二维数组**

```python
np.array([(1, 2, 3), (4, 5, 6)])
```



**3.创建全为 0 的二维数组**

```python
np.zeros((3, 3))
```



**4.创建全为 1 的三维数组**

```python
np.ones((2, 3, 4))
```



**5.创建一维和二维等差数组**

```python
np.arange(5)
np.arange(6).reshape(2, 3)
```



**6.创建单位矩阵（二维数组）**

```python
np.eye(3)
```



**7.创建等间隔一维数组**

```python
np.linspace(1, 10, num=6)
```



**8.创建二维随机数组**

```python
np.random.rand(2, 3)
```



**9.创建二维随机整数数组（数值小于 5）**

```python
np.random.randint(5, size=(2, 3))
```



**10.依据自定义函数创建数组**

```python
np.fromfunction(lambda i, j: i + j, (3, 3))
```

***

### 第二部分：矩阵运算

初始矩阵以下面代码为例：

```python
A = np.array([[1, 2],
             [3, 4]])
B = np.array([[5, 6],
               [7, 8]])
A, B
```



**11.矩阵元素间的四则运算**

注意：这里的A和B实际上是二维数组，这里的乘法和除法是对应位置的元素相乘和相除。

```py
A + B
A - B
A * B
A / B
```



**12.矩阵的乘法运算**

两种方法：一种是使用`dot`方法直接求解，另一种是将A和B利用`mat`方法转成矩阵然后再求解。

```python
np.dot(A, B)

# 如果使用 np.mat 将二维数组准确定义为矩阵，就可以直接使用 * 完成矩阵乘法计算
np.mat(A) * np.mat(B)
```



**13.数乘矩阵和矩阵除数**

```python
2 * A
A / 2
```



**14.矩阵的转置**

```python
A.T
```



**15.矩阵求逆**

```python
np.linalg.inv(A)
```

***

###  第三部分：数学函数

**16.三角函数**

下面以`sin`函数举例：

```python
print(a)

np.sin(a)
```



**17.以自然对数函数为底数的指数函数**

$y = e^x$

```python
np.exp(a)
```



**18.数组的方根的运算（开平方）**

```python
np.sqrt(a)
```



**19. 数组的方根的运算（立方）**

```python
np.power(a, 3)
```

***

### 第四部分：数组索引和切片

**20. 一维数组索引和切片**

```python
a = np.array([1, 2, 3, 4, 5])
print(a[0], a[-1])
print(a[0:2], a[:-1])
```



**21.二维数组索引和切片**

二维数组的切片用法：`[行元组,列元组]`

```python
a = np.array([(1, 2, 3), (4, 5, 6), (7, 8, 9)])
print(a[0], a[-1])
print(a[:, 1]) # 二维数组切片（取第 2 列）
a[1:3, :] # 二维数组切片（取第 2，3 行）
```

***

### 第五部分：数组形状操作

**22.生成随机的3*2数组**

```python
a = np.random.random((3, 2))
```



**23.查看数组形状**

```python
a.shape
```



**24.更改数组形状**

`reshape`方法：返回一个新数组，不会更改原始数组。

`resize`方法：会改变原来的数组。

```python
# reshape 并不改变原始数组
a.reshape(2, 3)

# resize 会改变原始数组
a.resize(2, 3)
```



**25.展平数组**

```python
a.ravel()
```



**26.垂直和水平拼合数组**

注意这里的垂直拼合行尺寸要一致，水平拼合列尺寸要一样。

```python
a = np.random.randint(10, size=(3, 3))
b = np.random.randint(10, size=(3, 3))

np.vstack((a, b))
np.hstack((a, b))
```



**27.沿横轴和纵轴分割数组**

相当于竖着切和横着切。

```python
np.hsplit(a, 3)
```

***

### 第六部分：数组排序和统计

以下面代码生成的数组为例:

```python
# 生成示例数组
a = np.array(([1, 4, 3], [6, 2, 9], [4, 7, 2]))
```



**28.数组排序**

`ndarray.sort(axis=-1,kind='quicksort',order=None)`

>使用方法：a.sort
>
>参数说明：
>
>axis：排序沿着数组的方向，0表示按行，1表示按列
>
>kind：排序的算法，提供了快排、混排、堆排
>
>order：不是指的顺序，以后用的时候再去分析这个
>
>作用效果：对数组a排序，排序后直接改变了a

```python
a.sort(axis=1)
```



**29.获取数组的最大最小值**

```python
np.max(a, axis=0)  # 返回每列最大值
np.min(a, axis=1)  # 返回每行最小值

np.argmax(a, axis=0) # 返回每列最大值索引
np.argmin(a, axis=1) # 返回每行最小值索引
```



**30.统计数组各列的中位数**

```python
# 继续使用上面的 a 数组
np.median(a, axis=0)
```



**31.统计数组各行的算术平均值**

```python
np.mean(a, axis=1)
```



**32. 统计数组各列的加权平均值**

```python
np.average(a, axis=0) # weights参数不指定时等价于mean方法
np.average(a, axis=0, weights=[1, 2, 3]) # 传入每一列的权重
```



**33.统计数组各行的方差**

```python
np.var(a, axis=1)
```



**34.统计数组各列的标准偏差**

```python
np.std(a, axis=0)
```

***

### 第七部分：进阶内容

**35. 创建一个 5x5 的二维数组，其中边界值为1，其余值为0**

```python
Z = np.ones((5, 5))
Z[1:-1, 1:-1] = 0
print(Z)
```



**36.使用数字 0 将一个全为 1 的 5x5 二维数组包围**

```python
Z = np.ones((5,5))
Z = np.pad(Z, pad_width=1, mode='constant', constant_values=0)
Z
```



**37.创建一个 5x5 的二维数组，并设置值 1, 2, 3, 4 落在其对角线下方**

`diag`方法：如果传入的是一维数组，则会生成一个二维数组，如果传入的是二维数组，则会得到他的对角线，k=0默认主对角线，大于0默认主对角线上第k条对角线，小于0为主对角线下第k条对角线。

```python
Z = np.diag(1 + np.arange(4), k=-1)
```



**38.创建一个 10x10 的二维数组，并使得 1 和 0 沿对角线间隔放置**

```python
Z = np.zeros((10, 10))
Z[1::2, ::2] = 1
Z[::2, 1::2] = 1
```



**39.创建一个 0-10 的一维数组，并将 (1, 9] 之间的数全部反转成负数**

```python
Z = np.arange(10)
Z[(1 < Z) & (Z <= 9)] *= -1
```



**40.找出两个一维数组中相同的元素**

```python  
Z1 = np.random.randint(0,10,10)
Z2 = np.random.randint(0,10,10)
print("Z1:", Z1)
print("Z2:", Z2)
np.intersect1d(Z1,Z2)
```



**41.使用 NumPy 打印昨天、今天、明天的日期**

```python
yesterday = np.datetime64('today', 'D') - np.timedelta64(1, 'D')
today     = np.datetime64('today', 'D')
tomorrow  = np.datetime64('today', 'D') + np.timedelta64(1, 'D')
print("yesterday: ", yesterday)
print("today: ", today)
print("tomorrow: ", tomorrow)
```



**42.使用五种不同的方法去提取一个随机数组的整数部分**

```python
Z = np.random.uniform(0,10,10)
print("原始值: ", Z)

print ("方法 1: ", Z - Z%1)
print ("方法 2: ", np.floor(Z))
print ("方法 3: ", np.ceil(Z)-1)
print ("方法 4: ", Z.astype(int))
print ("方法 5: ", np.trunc(Z))
```



**43.创建一个 5x5 的矩阵，其中每行的数值范围从 1 到 5**

```python
Z = np.zeros((5, 5))
Z += np.arange(1, 6)
```



**44.创建一个长度为 5 的等间隔一维数组，其值域范围从 0 到 1，但是不包括 0 和 1**

```python
Z = np.linspace(0,1,6,endpoint=False)[1:]
```



**45.创建一个长度为10的随机一维数组，并将其按升序排序**

```python
Z = np.random.random(10)
Z.sort()
Z
```



**46.创建一个 3x3 的二维数组，并将列按升序排序**

```python
Z = np.array([[7,4,3],[3,1,2],[4,2,6]])
print("原始数组: \n", Z)

Z.sort(axis=0)
```



**47.创建一个长度为 5 的一维数组，并将其中最大值替换成 0**

```python
Z = np.random.random(5)
print("原数组: ",Z)
Z[Z.argmax()] = 0
Z
```



**48.打印每个 NumPy 标量类型的最小值和最大值**

```python
for dtype in [np.int8, np.int32, np.int64]:
   print("The minimum value of {}: ".format(dtype), np.iinfo(dtype).min)
   print("The maximum value of {}: ".format(dtype),np.iinfo(dtype).max)
for dtype in [np.float32, np.float64]:
   print("The minimum value of {}: ".format(dtype),np.finfo(dtype).min)
   print("The maximum value of {}: ".format(dtype),np.finfo(dtype).max)
```



**49.将 `float32` 转换为整型**

```python
Z = np.arange(10, dtype=np.float32)
print(Z)

Z = Z.astype(np.int32, copy=False)
Z
```



**50.将随机二维数组按照第 3 列从上到下进行升序排列**

```python
Z = np.random.randint(0,10,(5,5))
print("排序前：\n",Z)

Z[Z[:,2].argsort()]
```



**51.从随机一维数组中找出距离给定数值（0.5）最近的数**

```python
Z = np.random.uniform(0,1,20)
print("随机数组: \n", Z)
z = 0.5
m = Z.flat[np.abs(Z - z).argmin()]

m
```



52.将二维数组的前两行进行顺序交换

```python
A = np.arange(25).reshape(5,5)
print(A)
A[[0,1]] = A[[1,0]]
print(A)
```



**53.找出随机一维数组中出现频率最高的值**

bincount从0开始计算到数组的最大值，返回一个数组，比如[0, 1, 3, 2, 4],表示原数组0出现了0次，1出现了1次，2出现了3次, 3出现了2次，4出现了4次。

```python
Z = np.random.randint(0,10,50)
print("随机一维数组:", Z)
np.bincount(Z).argmax()
```



**54.找出给定一维数组中非 0 元素的位置索引**

```python
Z = np.nonzero([1,0,2,0,1,0,4,0])
Z
```



**55.对于给定的 5x5 二维数组，在其内部随机放置 p 个值为 1 的数**

```python
p = 3

Z = np.zeros((5,5))
np.put(Z, np.random.choice(range(5*5), p, replace=False),1)

Z
```



**56.对于随机的 3x3 二维数组，减去数组每一行的平均值**

注：这里mean方法的`keepdims`属性表示是保持原来的维度。

```python
X = np.random.rand(3, 3)
print(X)

Y = X - X.mean(axis=1, keepdims=True)
Y
```



**57.获得二维数组点积结果的对角线数组**

方法一：直接点乘，然后用`diag`方法获得对角线的值。

```python
A = np.random.uniform(0,1,(3,3))
B = np.random.uniform(0,1,(3,3))

print(np.dot(A, B))

# 较慢的方法
np.diag(np.dot(A, B))
```

方法二：暂时还没搞明白原理，占坑

```python
# 较快的方法
print(A * B.T)
np.sum(A * B.T, axis=1)
```

方法三：暂时也没怎么深入搞明白，见文章：https://zhuanlan.zhihu.com/p/27739282

```python
# 更快的方法
np.einsum("ij, ji->i", A, B)
```



**58.找到随机一维数组中前 p 个最大值**

```python
Z = np.random.randint(1,100,100)
print(Z)

p = 5

Z[np.argsort(Z)[-p:]]
```



**59.计算随机一维数组中每个元素的 4 次方数值**

```python
x = np.random.randint(2,5,5)
print(x)

np.power(x,4)
```



**60.对于二维随机数组中各元素，保留其 2 位小数**

```python
Z = np.random.random((5,5))
print(Z)

np.set_printoptions(precision=2)
Z
```



**61.使用科学记数法输出 NumPy 数组**

```python
Z = np.random.random([5,5])
print(Z)

Z/1e3
```



**62.使用 NumPy 找出百分位数（25%，50%，75%）**

```python
a = np.arange(15)
print(a)

np.percentile(a, q=[25, 50, 75])
```



**63.找出数组中缺失值的总数及所在位置**

```python
# 生成含缺失值的 2 维数组
Z = np.random.rand(10,10)
Z[np.random.randint(10, size=5), np.random.randint(10, size=5)] = np.nan

print("缺失值总数: \n", np.isnan(Z).sum())
print("缺失值索引: \n", np.where(np.isnan(Z)))
```



**64.从随机数组中删除包含缺失值的行**

```python
# 沿用 79 题中的含缺失值的 2 维数组

Z[np.sum(np.isnan(Z), axis=1) == 0]
```



**65.统计随机数组中的各元素的数量**

`return_counts=True`表示显示每一个数出现的次数，为false则不显示。

```python
Z = np.random.randint(0,100,25).reshape(5,5)
print(Z)

np.unique(Z, return_counts=True) # 返回值中，第 2 个数组对应第 1 个数组元素的数量
```



66.**将数组中各元素按指定分类转换为文本值**

```python
# 指定类别如下
# 1 → 汽车
# 2 → 公交车
# 3 → 火车


Z = np.random.randint(1,4,10)
print(Z)

label_map = {1: "汽车", 2: "公交车", 3: "火车"}

[label_map[x] for x in Z]
```



**67.将多个 1 维数组拼合为单个 Ndarray**

```python
Z1 = np.arange(3)
Z2 = np.arange(3,7)
Z3 = np.arange(7,10)

Z = np.array([Z1, Z2, Z3])
print(Z)

np.concatenate(Z)
```



**68.打印各元素在数组中升序排列的索引**

```python
a = np.random.randint(100, size=10)
print('Array: ', a)

a.argsort()
```



**69.得到二维随机数组各行的最大值和最小值**

**方法一：**使用`amax`函数和`amin`函数

```python
Z = np.random.randint(1,100, [5,5])
print(Z)

print(np.amax(Z, axis=1))
print(np.amin(Z, axis=1))

```



**方法二：**使用`numpy.apply_along_axis(func, axis, arr, args*, kwargs**)`：

必选参数：`func`,`axis`,`arr`。其中func是我们自定义的一个函数，函数func(arr)中的arr是一个数组，函数的主要功能就是对数组里的每一个元素进行变换，得到目标的结果。

```python
print(np.apply_along_axis(np.max, arr=Z, axis=1))
print(np.apply_along_axis(np.min, arr=Z, axis=1))
```



**70.计算两个数组之间的欧式距离**

```python
a = np.array([1, 2])
b = np.array([7, 8])

# 数学计算方法,根据定义来求
print(np.sqrt(np.power((8-2), 2) + np.power((7-1), 2)))

# NumPy 计算
np.linalg.norm(b-a)
```



**71.打印复数的实部和虚部**

```python
a = np.array([1 + 2j, 3 + 4j, 5 + 6j])

print("实部：", a.real)
print("虚部：", a.imag)
```



**72.求解给出矩阵的逆矩阵并验证**

使用`numpy.linalg.inv`方法求逆矩阵，使用`numpy.allclose`方法来判断两个矩阵元素是否相等。

```python
matrix = np.array([[1., 2.], [3., 4.]])

inverse_matrix = np.linalg.inv(matrix)

# 验证原矩阵和逆矩阵的点积是否为单位矩阵
assert np.allclose(np.dot(matrix, inverse_matrix), np.eye(2))

inverse_matrix
```



**73.使用 Z-Score 标准化算法对数据进行标准化处理**

Z-Score标准化公式：$Z = \frac{X-\mathrm{mean}(X)}{\mathrm{std}(X)}$

下面这几个方法:`xmean`、`xstd`、`zscore`中如果axis不指定，则默认对整个数组求对应数值，返回一个实数，无维度,

如果加上`keepdims=True`,则可以保持其二维特性。

```python
# 根据公式定义函数
def zscore(x, axis = None):
    xmean = x.mean(axis=axis, keepdims=True)
    xstd  = np.std(x, axis=axis, keepdims=True)
    zscore = (x-xmean)/xstd
    return zscore

# 生成随机数据
Z = np.random.randint(10, size=(5,5))
print(Z)

zscore(Z)
```



**74.使用 Min-Max 标准化算法对数据进行标准化处理**

```python
# 根据公式定义函数
def min_max(x, axis=None):
    min = x.min(axis=axis, keepdims=True)
    max = x.max(axis=axis, keepdims=True)
    result = (x-min)/(max-min)
    return result

# 生成随机数据
Z = np.random.randint(10, size=(5,5))
print(Z)

min_max(Z)
```



**75.使用 L2 范数对数据进行标准化处理**

l2范数公式：$L_2 = \sqrt{x_1^2 + x_2^2 + \ldots + x_i^2}$

`np.linalg.norm`的参数：第一个参数为数组Z，也就是下面的v， 第二个参数是ord，表示所求的范数，默认为2，如果设置为1则求1范数，设置为`np.inf`则求无穷范数， keepdims保持二维性质。

axis=1表示按行向量处理，求多个行向量的范数

axis=0表示按列向量处理，求多个列向量的范数

axis=None表示矩阵范数。

```python
# 根据公式定义函数
def l2_normalize(v, axis=-1, order=2): 
    l2 = np.linalg.norm(v, ord = order, axis=axis, keepdims=True)
    l2[l2==0] = 1 #防止出现除0错误
    return v/l2

# 生成随机数据
Z = np.random.randint(10, size=(5,5))
print(Z)

l2_normalize(Z)
```



**76. 使用 NumPy 计算变量直接的相关性系数**

> 相关性系数取值从 `[-1, 1]` 变换，靠近 1 则代表正相关性较强，-1 则代表负相关性较强。结果如下所示，变量 A 与变量 A 直接的相关性系数为 `1`，因为是同一个变量。变量 A 与变量 C 之间的相关性系数为 `0.97`，说明相关性较强。

```python
Z = np.array([
    [1, 2, 1, 9, 10, 3, 2, 6, 7], # 特征 A
    [2, 1, 8, 3, 7, 5, 10, 7, 2], # 特征 B
    [2, 1, 1, 8, 9, 4, 3, 5, 7]]) # 特征 C

np.corrcoef(Z)
```



**77.使用 NumPy 计算矩阵的特征值和特征向量**

```python
M = np.matrix([[1,2,3], [4,5,6], [7,8,9]])

w, v = np.linalg.eig(M)

# w 对应特征值，v 对应特征向量
w, v
```

我们可以通过 `PAP'=M` 公式反算，验证是否能得到原矩阵。

P为特征向量，A为特征值的对角矩阵，P'为特征向量的逆。

```python
v * np.diag(w) * np.linalg.inv(v)
```



**78.使用 NumPy 计算 Ndarray 两相邻元素差值**

```python
Z = np.random.randint(1,10,10)
print(Z)

# 计算 Z 两相邻元素差值
print(np.diff(Z, n=1))

# 重复计算 2 次
print(np.diff(Z, n=2))

# 重复计算 3 次
print(np.diff(Z, n=3))
```



**79.使用 NumPy 将 Ndarray 相邻元素依次累加**

```python
Z = np.random.randint(1,10,10)
print(Z)

"""
[第一个元素, 第一个元素 + 第二个元素, 第一个元素 + 第二个元素 + 第三个元素, ...]
"""
np.cumsum(Z)
```



80.使用 NumPy 按列(行)连接两个数组

```python
M1 = np.array([1, 2, 3])
M2 = np.array([4, 5, 6])

np.c_[M1, M2]  # 按列连接
np.r_[M1, M2]  # 按行连接
```



**81.使用 NumPy 打印九九乘法表**

`np.fromfunction`方法：以函数的方式生成数组，第一个参数为函数，第二个为函数的参数，即数组形状。

```python
np.fromfunction(lambda i, j: (i + 1) * (j + 1), (9, 9))
```



**82.使用 NumPy 将实验楼 LOGO 转换为 Ndarray 数组**

```python
from io import BytesIO
from PIL import Image
import PIL, requests

# 通过链接下载图像
URL = 'https://static.shiyanlou.com/img/logo-black.png'
response = requests.get(URL)

# 将内容读取为图像
I = Image.open(BytesIO(response.content))

# 将图像转换为 Ndarray
shiyanlou = np.asarray(I)
shiyanlou
```



```python
# 将转换后的 Ndarray 重新绘制成图像
from matplotlib import pyplot as plt
%matplotlib inline

plt.imshow(shiyanlou)
plt.show()
```











