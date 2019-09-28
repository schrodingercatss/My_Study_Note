### 0x00：pandas的数据类型

> Pandas 的数据类型主要有以下几种，它们分别是：Series（一维数组），DataFrame（二维数组），Panel（三维数组），Panel4D（四维数组），PanelND（更多维数组）。其中 Series 和 DataFrame 应用的最为广泛，几乎占据了使用频率 90% 以上。

我们主要使用它的`Series`和`DataFrame`两种数据结构。

***

### 0x01：pandas的Series基础

[<i class="fa fa-external-link-square" aria-hidden="true"> Series</i>](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.html) 是 Pandas 中最基本的一维数组形式。其可以储存整数、浮点数、字符串等类型的数据。Series 基本结构如下：

```python
pandas.Series(data=None, index=None)
```

其中，`data` 可以是字典，或者NumPy 里的 ndarray 对象等。`index` 是数据索引，索引是 Pandas 数据结构中的一大特性，它主要的功能是帮助我们更快速地定位数据。



下面是一个`demo`， 使用字典来创建`Series`，字典的key值是它的行索引，value值是它的值，该 Series 的数据值是 10, 20, 30，索引为 a, b, c，数据值的类型默认识别为 `int64`。你可以通过 `type` 来确认 `s` 的类型。

```python
s = pd.Series({'a': 10, 'b': 20, 'c': 30})  # 创建一个Series
```



由于 Pandas 基于 NumPy 开发。那么 NumPy 的数据类型 `ndarray` 多维数组自然就可以转换为 Pandas 中的数据。而 Series 则可以基于 NumPy 中的一维数据转换，我们给出了 NumPy 生成的一维随机数组，最终得到的 Series 索引默认从 0 开始，而数值类型为 `float64`。

```python
s = pd.Series(np.random.randn(5))
```

***

### 0x02：pandas的DataFrame基础

> DataFrame 是 Pandas 中最为常见、最重要且使用频率最高的数据结构。DataFrame 和平常的电子表格或 SQL 表结构相似。你可以把 DataFrame 看成是 Series 的扩展类型，它仿佛是由多个 Series 拼合而成。它和 Series 的直观区别在于，数据不但具有行索引，且具有列索引。

![img](https://doc.shiyanlou.com/courses/uid214893-20190531-1559284057250)



DataFrame 基本结构如下：

```python
pandas.DataFrame(data=None, index=None, columns=None)
```

区别于 Series，其增加了 `columns` 列索引。DataFrame 可以由以下多个类型的数据构建：

- 一维数组、列表、字典或者 Series 字典。
- 二维或者结构化的 `numpy.ndarray`。
- 一个 Series 或者另一个 DataFrame。



使用一个由 Series 组成的字典来构建 DataFrame字典来初始化一个`DataFrame`:

```python
df = pd.DataFrame({'one': pd.Series([1, 2, 3]),
                   'two': pd.Series([4, 5, 6])})
```



当不指定索引时，DataFrame 的索引同样是从 0 开始。我们也可以直接通过一个列表构成的字典来生成 DataFrame。

```python
df = pd.DataFrame({'one': [1, 2, 3],
                   'two': [4, 5, 6]})
```



或者反过来，由带字典的列表生成 DataFrame。

```python
df = pd.DataFrame([{'one': 1, 'two': 4},
                   {'one': 2, 'two': 5},
                   {'one': 3, 'two': 6}])
```



NumPy 的多维数组非常常用，同样可以基于二维数值来构建一个 DataFrame，下面是生成一个不超过5的2行4列整型矩阵。

```python
pd.DataFrame(np.random.randint(5, size=(2, 4)))
```



根据上述对比我们可以得知，`Series`和`DataFrame`的本质区别就是`DataFrame`增加了一个列索引，

***

### 0x03: 数据读取操作

我们想要使用 Pandas 来分析数据，那么首先需要读取数据。大多数情况下，数据都来源于外部的数据文件或者数据库。Pandas 提供了一系列的方法来读取外部数据，非常全面。下面，我们以最常用的 CSV 数据文件为例进行介绍。

读取数据 CSV 文件的方法是 `pandas.read_csv()`，你可以直接传入一个相对路径，或者是网络 URL。

```python
df = pd.read_csv("https://labfile.oss.aliyuncs.com/courses/906/los_census.csv")
```



由于 CSV 存储时是一个二维的表格，那么 Pandas 会自动将其读取为 DataFrame 类型。

现在你应该就明白了，DataFrame 是 Pandas 构成的核心。一切的数据，无论是外部读取还是自行生成，我们都需要先将其转换为 Pandas 的 DataFrame 或者 Series 数据类型。实际上，大多数情况下，这一切都是设计好的，无需执行额外的转换工作。

`pd.read_` 前缀开始的方法还可以读取各式各样的数据文件，且支持连接数据库。这里，我们不再依次赘述，你可以阅读 [<i class="fa fa-external-link-square" aria-hidden="true"> 官方文档相应章节</i>](https://pandas.pydata.org/pandas-docs/stable/reference/io.html) 熟悉这些方法以及搞清楚这些方法包含的参数。

**为什么要将数据转换成Series或者DataFrame呢？**

> 实际上，因为 Pandas 针对数据操作的全部方法都是基于 Pandas 支持的数据结构设计的。也就是说，只有 Series 或者 DataFrame 才能使用 Pandas 提供的方法和函数进行处理。所以，学习真正数据处理方法之前，我们需要将数据转换生成为 Series 或 DataFrame 类型。

***

### 0x04： 数据结构的基本操作

**我们以DataFrame为例，Series类似。**

通过上面的内容，我们已经知道一个 DataFrame 结构大致由 3 部分组成，它们分别是列名称、索引和数据。

![img](https://doc.shiyanlou.com/courses/uid214893-20190531-1559286555222)

上面，我们已经读取了一个外部数据，这是洛杉矶的人口普查数据。有些时候，我们读取的文件很大。如果全部输出预览这些文件，既不美观，又很耗时。还好，Pandas 提供了 `head()` 和 `tail()` 方法，它可以帮助我们只预览一小块数据。

```python
df.head()  # 默认显示前5条,括号内可指定查阅条数
df.tail(7) # 默认显示后5条，指定显示后7条
```



Pandas 还提供了统计和描述性方法，方便你从宏观的角度去了解数据集。`describe()` 相当于对数据集进行概览，会输出该数据集每一列数据的计数、最大值、最小值等。

```python
df.describe()
```



Pandas 基于 NumPy 开发，所以任何时候你都可以通过 `.values` 将 DataFrame 转换为 NumPy 数组。

```python
df.values
```



这也就说明了，你可以同时使用 Pandas 和 NumPy 提供的 API 对同一数据进行操作，并在二者之间进行随意转换。这就是一个非常灵活的工具生态圈。



除了 `.values`，DataFrame 支持的常见属性可以通过 [<i class="fa fa-external-link-square" aria-hidden="true"> 官方文档相应章节</i>](https://pandas.pydata.org/pandas-docs/stable/reference/frame.html#attributes-and-underlying-data) 查看。其中常用的有：

```python
df.index  # 查看索引
df.columns  # 查看列名
df.shape  # 查看形状
```

***

### 0x05：数据的选择操作

在数据预处理过程中，我们往往会对数据集进行切分，只将需要的某些行、列，或者数据块保留下来，输出到下一个流程中去。这也就是所谓的数据选择，或者数据索引。

由于 Pandas 的数据结构中存在索引、标签，所以我们可以通过多轴索引完成对数据的选择。



**基于索引的数字选择：**

当我们新建一个 DataFrame 之后，如果未自己指定行索引或者列对应的标签，那么 Pandas 会默认从 0 开始以数字的形式作为行索引，并以数据集的第一行作为列对应的标签。其实，这里的「列」也有数字索引，默认也是从 0 开始，只是未显示出来。

所以，我们首先可以基于数字索引对数据集进行选择。这里用到的 Pandas 中的 [`.iloc`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.iloc.html#pandas.DataFrame.iloc) 方法。该方法可以接受的类型有：

1.  整数。例如：`5`
2.  整数构成的列表或数组。例如：`[1, 2, 3]`
3.  布尔数组。
4.  可返回索引值的函数或参数。

```python
df.iloc[:3]  # 选择前3行
df.iloc[5] # 选择特定的一行
```

`df.iloc[]` 的 `[[行]，[列]]` 里面可以同时接受行和列的位置，但如果你直接键入 `df.iloc[1, 3, 5]` 就会报错。

```python
df.iloc[[1, 3, 5]]  # 选择1,3,5行
df.iloc[:, 1:4]  # 选择2到4列
```

这里选择 2-4 列，输入的却是 `1:4`。这和 Python 或者 NumPy 里面的切片操作非常相似。既然我们能定位行和列，那么只需要组合起来，我们就可以选择数据集中的任何数据了。



 **基于标签名称选择：**

除了根据数字索引选择，还可以直接根据标签对应的名称选择。这里用到的方法和上面的 `iloc` 很相似，少了个 `i` 为 [`df.loc[]`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.loc.html#pandas.DataFrame.loc)。

`df.loc[]` 可以接受的类型有：

1.  单个标签。例如：`2` 或 `'a'`，这里的 `2` 指的是标签而不是索引位置。
2.  列表或数组包含的标签。例如：`['A', 'B', 'C']`。
3.  切片对象。例如：`'A':'E'`，注意这里和上面切片的不同支持，首尾都包含在内。
4.  布尔数组。
5.  可返回标签的函数或参数。

```python
df.loc[0:2]  # 选择前3行
df.loc[[0, 2, 4]]  # 选择1，3， 5行
df.loc[:, 'Total Population':'Total Males']  # 选择2-4列
df.loc[[0, 2], 'Median Age':]  # 选择1-3行和Median Age 后面的列
```



**两者区别：**

`iloc`是用于数字行索引的选择，而`loc`是用于标签的选择，通常不能使用行索引，当且仅当行索引是数字时才能使用，否则要使用字符串等值来进行行索引选择。

***

###  0x06：数据删减

虽然我们可以通过数据选择方法从一个完整的数据集中拿到我们需要的数据，但有的时候直接删除不需要的数据更加简单直接。Pandas 中，以 `.drop` 开头的方法都与数据删减有关。

[`DataFrame.drop`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.drop.html#pandas.DataFrame.drop) 可以直接去掉数据集中指定的列和行。一般在使用时，我们指定 `labels` 标签参数，然后再通过 `axis` 指定按列或按行删除即可。当然，你也可以通过索引参数删除数据，具体查看官方文档。

```python
df.drop(labels=['Median Age', 'Total Males'], axis=1)
```



[`DataFrame.drop_duplicates`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.drop_duplicates.html#pandas.DataFrame.drop_duplicates) 则通常用于数据去重，即剔除数据集中的重复值。使用方法非常简单，指定去除重复值规则，以及 `axis` 按列还是按行去除即可。

```python
df.drop_duplicates()
```



除此之外，另一个用于数据删减的方法 [`DataFrame.dropna`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.dropna.html#pandas.DataFrame.dropna) 也十分常用，其主要的用途是删除缺少值，即数据集中空缺的数据列或行。

```python
df.dropna()
```

***

### 0x07：数据填充

既然提到了数据删减，反之则可能会遇到数据填充的情况。而对于一个给定的数据集而言，我们一般不会乱填数据，而更多的是对缺失值进行填充。

在真实的生产环境中，我们需要处理的数据文件往往没有想象中的那么美好。其中，很大几率会遇到的情况就是缺失值。缺失值主要是指数据丢失的现象，也就是数据集中的某一块数据不存在。除此之外、存在但明显不正确的数据也被归为缺失值一类。例如，在一个时间序列数据集中，某一段数据突然发生了时间流错乱，那么这一小块数据就是毫无意义的，可以被归为缺失值。

 **检测缺失值**

Pandas 为了更方便地检测缺失值，将不同类型数据的缺失均采用 `NaN` 标记。这里的 NaN 代表 Not a Number，它仅仅是作为一个标记。例外是，在时间序列里，时间戳的丢失采用 `NaT` 标记。

Pandas 中用于检测缺失值主要用到两个方法，分别是：[`isna()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.isna.html#pandas.DataFrame.isna) 和 [`notna()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.notna.html#pandas.DataFrame.notna)，故名思意就是「是缺失值」和「不是缺失值」。默认会返回布尔值用于判断。



下面我们人为生成一组缺失值：

```python
df = pd.DataFrame(np.random.rand(9, 5), columns=list('ABCDE'))
# 插入 T 列，并打上时间戳
df.insert(value=pd.Timestamp('2017-10-1'), loc=0, column='Time')
# 将 1, 3, 5 列的 1，3，5 行置为缺失值
df.iloc[[1, 3, 5, 7], [0, 2, 4]] = np.nan
# 将 2, 4, 6 列的 2，4，6 行置为缺失值
df.iloc[[2, 4, 6, 8], [1, 3, 5]] = np.nan
```

然后，通过 [`isna()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.isna.html#pandas.DataFrame.isna) 或 [`notna()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.notna.html#pandas.DataFrame.notna) 中的一个即可确定数据集中的缺失值。

```python
df.isna()
```

上面已经对缺省值的产生、检测进行了介绍。实际上，面对缺失值一般就是填充和剔除两项操作。填充和清除都是两个极端。如果你感觉有必要保留缺失值所在的列或行，那么就需要对缺失值进行填充。如果没有必要保留，就可以选择清除缺失值。



其中，缺失值剔除的方法 `dropna()` 已经在上面介绍过了。下面来看一看填充缺失值 `fillna()` 方法。

首先，我们可以用相同的标量值替换 `NaN`，比如用 `0`。

```python
df.fillna(0)
```

除了直接填充值，我们还可以通过参数，将缺失值前面或者后面的值填充给相应的缺失值。例如使用缺失值前面的值进行填充：

```python
df.fillna(method='pad')
```

或者是后面的值：

```python
df.fillna(method='bfill')
```

```
	Time	A	B	C	D	E
0	2017-10-01	0.482566	0.856572	0.684022	0.906754	0.217237
1	2017-10-01	0.040800	0.461499	0.802284	0.202109	0.624800
2	2017-10-01	0.903368	0.461499	0.494029	0.202109	0.457622
3	2017-10-01	0.903368	0.422430	0.494029	0.777427	0.457622
4	2017-10-01	0.426328	0.422430	0.154653	0.777427	0.134179
5	2017-10-01	0.426328	0.802082	0.154653	0.578898	0.134179
6	2017-10-01	0.400250	0.802082	0.439411	0.578898	0.341871
7	2017-10-01	0.400250	0.466289	0.439411	0.032090	0.341871
8	2017-10-01	NaN	        0.466289	NaN	        0.032090	NaN
```

最后一行由于没有对于的后序值，自然继续存在缺失值。



上面的例子中，我们的缺失值是间隔存在的。那么，如果存在连续的缺失值是怎样的情况呢？试一试。首先，我们将数据集的第 2，4 ，6 列的第 3，5 行也置为缺失值。

```python
df.iloc[[3, 5], [1, 3, 5]] = np.nan
```

然后来正向填充：

```python
df.fillna(method='pad')
```

```

Time	A	B	C	D	E
0	2017-10-01	0.482566	0.856572	0.684022	0.906754	0.217237
1	2017-10-01	0.040800	0.856572	0.802284	0.906754	0.624800
2	2017-10-01	0.040800	0.461499	0.802284	0.202109	0.624800
3	2017-10-01	0.903368	0.461499	0.494029	0.202109	0.457622
4	2017-10-01	0.903368	0.422430	0.494029	0.777427	0.457622
5	2017-10-01	0.426328	0.422430	0.154653	0.777427	0.134179
6	2017-10-01	0.426328	0.802082	0.154653	0.578898	0.134179
7	2017-10-01	0.400250	0.802082	0.439411	0.578898	0.341871
8	2017-10-01	0.400250	0.466289	0.439411	0.032090	0.341871
```

可以看到，连续缺失值也是按照前序数值进行填充的，并且完全填充。这里，我们可以通过 `limit=` 参数设置连续填充的限制数量。

```python
df.fillna(method='pad', limit=1)  # 最多填充一项
```

```
    Time	    A	        B           C	        D	        E
0	2017-10-01	0.482566	0.856572	0.684022	0.906754	0.217237
1	2017-10-01	0.040800	0.856572	0.802284	0.906754	0.624800
2	2017-10-01	0.040800	0.461499	0.802284	0.202109	0.624800
3	2017-10-01	NaN	        0.461499	NaN	        0.202109	NaN
4	2017-10-01	NaN	        0.422430	NaN	        0.777427	NaN
5	2017-10-01	NaN	        0.422430	NaN	        0.777427	NaN
6	2017-10-01	NaN	        0.802082	NaN	        0.578898	NaN
7	2017-10-01	0.400250	0.802082	0.439411	0.578898	0.341871
8	2017-10-01	0.400250	0.466289	0.439411	0.032090	0.341871
```

除了上面的填充方式，还可以通过 Pandas 自带的求平均值方法等来填充特定列或行。举个例子：

```python
df.fillna(df.mean()['C':'E'])
```

***

**插值填充**

插值是数值分析中一种方法。简而言之，就是借助于一个函数（线性或非线性），再根据已知数据去求解未知数据的值。插值在数据领域非常常见，它的好处在于，可以尽量去还原数据本身的样子。

我们可以通过 [`interpolate()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.interpolate.html#pandas.DataFrame.interpolate) 方法完成线性插值。当然，其他一些插值算法可以阅读官方文档了解。

```python
# 生成一个 DataFrame
df = pd.DataFrame({'A': [1.1, 2.2, np.nan, 4.5, 5.7, 6.9],
                   'B': [.21, np.nan, np.nan, 3.1, 11.7, 13.2]})
```

对于上面存在的缺失值，如果通过前后值，或者平均值来填充是不太能反映出趋势的。这时候，插值最好使。我们用默认的线性插值试一试。

```python
df_interpolate = df.interpolate()
df_interpolate
```

下图展示了插值后的数据，明显看出插值结果符合数据的变化趋势。如果按照前后数据顺序填充，则无法做到这一点。

![image](https://doc.shiyanlou.com/document-uid214893labid3378timestamp1501655359105.png)

对于 [`interpolate()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.interpolate.html#pandas.DataFrame.interpolate) 支持的插值算法，也就是 `method=`。下面给出几条选择的建议：

1.  如果你的数据增长速率越来越快，可以选择 `method='quadratic'`二次插值。
2.  如果数据集呈现出累计分布的样子，推荐选择 `method='pchip'`。
3.  如果需要填补缺省值，以平滑绘图为目标，推荐选择 `method='akima'`。

当然，最后提到的 `method='akima'`，需要你的环境中安装了 Scipy 库。除此之外，`method='barycentric'` 和 `method='pchip'` 同样也需要 Scipy 才能使用。

***

### 0x08：数据可视化

NumPy，Pandas，Matplotlib 构成了一个完善的数据分析生态圈，所以 3 个工具的兼容性也非常好，甚至共享了大量的接口。当我们的数据是以 DataFrame 格式呈现时，可以直接使用 Pandas 提供的 [`DataFrame.plot`](https://pandas.pydata.org/pandas-docs/stable/reference/frame.html#plotting) 方法调用 Matplotlib 接口绘制常见的图形。

例如，我们使用上面插值后的数据 `df_interpolate` 绘制线形图。

```python
df_interpolate.plot()
```

其他样式的图形也很简单，指定 `kind=` 参数即可。

```python
df_interpolate.plot(kind='bar')
```

更多的图形样式和参数，阅读官方文档中的详细说明。Pandas 绘图虽然不可能做到 Matplotlib 的灵活性，但是其简单易用，适合于数据的快速呈现和预览。

***

### 0x09：总结

由于 Pandas 包含的内容实在太多，除了阅读完整的官方文档，很难做到通过一个实验或者一个课程进行全面了解。当然，本课程的目的是带大家熟悉 Pandas 的常用基础方法，至少你大致清楚了 Pandas 是什么，能干什么。

除了上面提到的一些方法和技巧，实际上 Pandas 常用的还有：

- [<i class="fa fa-external-link-square" aria-hidden="true"> 数据计算</i>](https://pandas.pydata.org/pandas-docs/stable/reference/frame.html#binary-operator-functions)，例如：`DataFrame.add` 等。
- [<i class="fa fa-external-link-square" aria-hidden="true"> 数据聚合</i>](https://pandas.pydata.org/pandas-docs/stable/reference/frame.html#function-application-groupby-window)，例如：`DataFrame.groupby` 等。
- [<i class="fa fa-external-link-square" aria-hidden="true"> 统计分析</i>](https://pandas.pydata.org/pandas-docs/stable/reference/frame.html#computations-descriptive-stats)，例如：`DataFrame.abs` 等。
- [<i class="fa fa-external-link-square" aria-hidden="true"> 时间序列</i>](https://pandas.pydata.org/pandas-docs/stable/reference/frame.html#time-series-related)，例如：`DataFrame.shift` 等。

