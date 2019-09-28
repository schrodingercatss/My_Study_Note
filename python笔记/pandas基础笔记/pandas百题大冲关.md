## pandas百题大冲关

***

###第一部分：pandas简介

Pandas 是基于 NumPy 的一种数据处理工具，该工具为了解决数据分析任务而创建。Pandas 纳入了大量库和一些标准的数据模型，提供了高效地操作大型数据集所需的函数和方法。

Pandas 的数据结构：Pandas 主要有 Series（一维数组），DataFrame（二维数组），Panel（三维数组），Panel4D（四维数组），PanelND（更多维数组）等数据结构。其中 Series 和 DataFrame 应用的最为广泛。

- Series 是一维带标签的数组，它可以包含任何数据类型。包括整数，字符串，浮点数，Python 对象等。Series 可以通过标签来定位。
- DataFrame 是二维的带标签的数据结构。我们可以通过标签来定位数据。这是 NumPy 所没有的。

***

#### 第二部分：Series的基本操作

**1.从列表创建Series**

```python
import pandas as pd
arr = [0, 1, 2, 3, 4]
s1 = pd.Series(arr)  # 如果不指定索引，则默认从 0 开始
s1
```



**2.从 Ndarray 创建 Series**

```python
import numpy as np
n = np.random.randn(5)  # 创建一个随机 Ndarray 数组

index = ['a', 'b', 'c', 'd', 'e']
s2 = pd.Series(n, index=index)
s2
```



**3.从字典创建 Series**

```python
d = {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}
s3 = pd.Series(d)
s3
```



**4.修改 Series 索引**

```python
print(s1)  # 以 s1 为例

s1.index = ['A', 'B', 'C', 'D', 'E']  # 修改后的索引
s1
```



**5.Series 纵向拼接**

```python
s4 = s3.append(s1)  # 将 s1 拼接到 s3
s4
```



**6.Series 按指定索引删除元素**

```python
print(s4)
s4 = s4.drop('e')  # 删除索引为 e 的值
s4
```



**7.Series 修改指定索引元素**

```python
s4['A'] = 6  # 修改索引为 A 的值 = 6
s4
```



**8.Series 按指定索引查找元素**

```python
s4['B']
```



**9.Series 切片操作**

对s4的前3个元素进行访问：

```python
s4[:3]
```



**10.Series的四则运算**

Series 的四则运算是按照索引计算，如果索引不同则填充为 `NaN`（空值）。

注：如果除法运算除数为0，最终不会抛出异常，而是得到`np.inf`。

```python
s4.add(s3)
s4.sub(s3)
s4.mul(s3)
s4.div(s3)
```



**11.Series 求中位数**

```python
s4.median()
```



**12.Series求和**

注：在求sum的时候，NAN被视为0，如果有字母，则会抛出异常。

```python
s4.sum()
```



**13.Series求最大(小)值**

注：在求最值的时候，NAN被视为0，如果有字母，则会抛出异常。

```python
s4.max()
s4.min()
```

***

### 第三部分：DataFrame基本操作

**14.通过NumPy数组创建Dataframe**

其中第一个参数为元素值，第二个参数`index`为行索引，`columns`为列索引。

```python
dates = pd.date_range('today', periods=6)
num_arr = np.random.randn(6, 4)
columns = ['A', 'B', 'C', 'D']
df1 = pd.DataFrame(num_arr, index=dates, columns=columns)
df1
```





**15.通过字典数组创建 DataFrame**

其中字典的`key`一般为一个字符串，代表列索引，`values`为一个列表或者元素，代表元素值。

```python
data = {'animal': ['cat', 'cat', 'snake', 'dog', 'dog', 'cat', 'snake', 'cat', 'dog', 'dog'],
        'age': [2.5, 3, 0.5, np.nan, 5, 2, 4.5, np.nan, 7, 3],
        'visits': [1, 3, 2, 3, 2, 3, 1, 1, 2, 1],
        'priority': ['yes', 'yes', 'no', 'yes', 'no', 'no', 'no', 'yes', 'no', 'no']}

labels = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']
df = pd.DataFrame(data, index=labels)
df
```



**16.查看DataFrame的数据类型**

会打印每一列的数据类型和所有类型中最基本的类型。

```python
df2.dtypes
```



**17.预览 DataFrame 的前 (后）5 行数据**

```python
df2.head() # 默认显示前5行数据
df2.head(10) # 传入的参数为显示前n行数据

df2.tail() # 默认显示后5行数据
df2.tail(3) # 传入的参数为显示后n行数据
```



**18.查看 DataFrame 的索引**

```python
df2.index
```



**19.查看 DataFrame 的列名**

```python
df2.columns
```



**20.查看 DataFrame 的数值**

```python
df2.values
```



**21.查看 DataFrame 的统计数据**

```python
df2.describe()
```



**22.DataFrame 转置操作**

转置操作不仅元素值会被转置，行索引和列索引也会被转置。

```python
df2.T
```



**23.对 DataFrame 进行按列排序**

```python
df2.sort_values(by='age')  # 按 age 升序排列
```



**24.对 DataFrame 数据切片**

单个索引是针对行来进行索引的，下面代表取第2行和第3行。

```python
df2[1:3]
```

```python
df2[1:3]
```



**25.对 DataFrame 通过标签查询（单列）**

```python
df2['age']
df.age  # 与上述等价
```



**26.对 DataFrame 通过标签查询（多列）**

```python
df2[['age', 'animal']]  # 传入一个列名组成的列表
```



**27.对 DataFrame 通过位置查询**

`iloc`传入一个索引表示对行进行索引。

**隐式索引：**iloc（integer_location），只能传入整数。

```python
df2.iloc[1:3]  # 查询 2，3 行
```



**28.DataFrame 副本拷贝**

```python
# 生成 DataFrame 副本，方便数据集被多个不同流程使用
df3 = df2.copy()
df3
```



**29.判断 DataFrame 元素是否为空**

```python
df3.isnull()  # 如果为空则返回为 True
```



**30.添加列数据**

```python
num = pd.Series([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], index=df3.index)

df3['No.'] = num  # 添加以 'No.' 为列名的新数据列
df3
```



**31.根据 DataFrame 的下标值进行更改**

`iat:`类似于iloc使用隐式索引访问某个元素

```python
# 修改第 2 行与第 2 列对应的值 3.0 → 2.0
df3.iat[1, 1] = 2  # 索引序号从 0 开始，这里为 1, 1
df3
```



**32.根据 DataFrame 的标签对数据进行修改**

**显示索引：**loc,第一个参数为index，第二个为columns,可传入切片或者单个值。

```python
df3.loc['f', 'age'] = 1.5
df3
```



**33.DataFrame 求平均值操作**

```python
df3.mean()
```



**34.对 DataFrame 中任意列做求和操作**

```python
df3['visits'].sum()
```



**35.将字符串转化为小(大)写字母**

```python
string = pd.Series(['A', 'B', 'C', 'Aaba', 'Baca',
                    np.nan, 'CABA', 'dog', 'cat'])
print(string)
string.str.lower()

string.str.upper()
```



#### 对缺失值进行操作：

**36.DataFrame 缺失值进行填充**

对缺失值进行填充，`value`参数表示填充的值。

```python
df4 = df3.copy()
print(df4)
df4.fillna(value=3)
```



**37.删除存在缺失值的行**

```python
df5 = df3.copy()
print(df5)
df5.dropna(how='any')  # 任何存在 NaN 的行都将被删除
```



**38.DataFrame 按指定列对齐**

```python
left = pd.DataFrame({'key': ['foo1', 'foo2'], 'one': [1, 2]})
right = pd.DataFrame({'key': ['foo2', 'foo3'], 'two': [4, 5]})

print(left)
print(right)

# 按照 key 列对齐连接，只存在 foo2 相同，所以最后变成一行
pd.merge(left, right, on='key')
```



### 对文件进行操作

**39.CSV 文件写入**

```python
df3.to_csv('animal.csv')
print("写入成功.")
```



**40.CSV 文件读取**

```python
df_animal = pd.read_csv('animal.csv')
df_animal
```



**41.Excel 写入操作**

```python
df3.to_excel('animal.xlsx', sheet_name='Sheet1')
print("写入成功.")
```



**42.Excel 读取操作**

`index_col`：指定列为索引列，默认None列（0索引）用作DataFrame的行标签。

**na_values** : 标量，字符串，列表类，或字典，默认None

```python
pd.read_excel('animal.xlsx', 'Sheet1', index_col=None, na_values=['NA'])
```



### 时间操作

**44.建立一个以 2018 年每一天为索引，值为随机数的 Series**

`pd.date_range`方法，`start`属性表示开始时间，`end`属性表示结束时间，`freq`属性表示每隔多少时间取一次，D表示天，M表示月，Y表示年，S表示秒，min表示分，h表示时。

```python
dti = pd.date_range(start='2018-01-01', end='2018-12-31', freq='D')
s = pd.Series(np.random.rand(len(dti)), index=dti)
s
```



**45.统计`s` 中每一个周三对应值的和**

上述生成的Series对象中有很多属性，我们使用它的weekday属性结合的布尔表达式来得到。

```python
# 周一从 0 开始
s[s.index.weekday == 2].sum()
```



**46.统计`s`中每个月值的平均值**

在pandas里对时序的频率的调整称之重新采样，即从一个时频调整为另一个时频的操作,我们可以用`resample`方法来实现，参数为`freq`，与44中的参数对应。

```python
s.resample('M').mean()
```



**47.将 Series 中的时间进行转换（秒转分钟）**

```python
s = pd.date_range('today', periods=100, freq='S')

ts = pd.Series(np.random.randint(0, 500, len(s)), index=s)

ts.resample('Min').sum()
```



**48.UTC 世界时间标准**

```python
s = pd.date_range('today', periods=1, freq='D')  # 获取当前时间
ts = pd.Series(np.random.randn(len(s)), s)  # 随机数值
ts_utc = ts.tz_localize('UTC')  # 转换为 UTC 时间
ts_utc
```



**49.转换为上海所在时区**

```python
ts_utc.tz_convert('Asia/Shanghai')
```



**50.不同时间表示方式的转换**

**Timestamp**:是从Python标准库的datetime类继承过来的，表示时间轴上的一个时刻。他不含时区信息的本地时间，但提供了方便的时区转换功能。

**Period**：表示一个标准的时间段。例如某年、某月、某日、某小时等。时间的长短由freq决定。

`to_period`方法:时间戳timestamp转为时期period，默认显示格式为`YYYY-MM`。

`to_timestamp`方法：可以将时期period转成时间戳timestamp。

```python
rng = pd.date_range('1/1/2018', periods=5, freq='M')
ts = pd.Series(np.random.randn(len(rng)), index=rng)
print(ts)
ps = ts.to_period()
print(ps)
ps.to_timestamp()
```



### Series多重索引

51.构建一个 `letters = ['A', 'B', 'C']` 和 `numbers = list(range(10))`为索引，值为随机数的多重索引 Series。

```python
letters = ['A', 'B', 'C']
numbers = list(range(10))

mi = pd.MultiIndex.from_product([letters, numbers])  # 设置多重索引
s = pd.Series(np.random.rand(30), index=mi)  # 随机数
s
```



**52.多重索引 Series 查询**

```python
# 查询索引为 1，3，6 的值
s.loc[:, [1, 3, 6]]
```



**53.多重索引 Series 切片**

下面两种切片方法等价。

```python
s.loc[pd.IndexSlice[:'B', 5:]]
print(s.loc[:'B', 5:])
```



### DataFrame 多重索引

**54.根据多重索引创建 DataFrame**

创建一个以 `letters = ['A', 'B']` 和 `numbers = list(range(6))`为索引，值为随机数据的多重索引 DataFrame。

```python
frame = pd.DataFrame(np.arange(12).reshape(6, 2),
                     index=[list('AAABBB'), list('123123')],
                     columns=['hello', 'shiyanlou'])
frame
```



**55.多重索引设置列名称**

```python
frame.index.names = ['first', 'second']
frame
```



  **56.DataFrame 多重索引分组求和**

```python
frame.groupby('first').sum()
frame.groupby('second').sum()
```



**57.DataFrame 行列名称转换**

`stack()`方法：是将原来的列索引转成了最内层的行索引，这里是多层次索引，其中AB索引对应第三层，即最内层索引。

![20180704191137494.png](https://i.loli.net/2019/09/23/tZzwmRk8FoBOTnp.png)

```python
print(frame)
frame.stack()
```



**58.DataFrame 索引转换**

`unstack()`是`stack()`的逆操作，这里把最内层的行索引还原成了列索引。但是unstack()中有一个参数可以指定旋转第几层索引，比如unstack(0)就是把第一层行索引转成列索引，但默认的是把最内层索引转层列索引。

![20180704191457916.png](https://i.loli.net/2019/09/23/JHVGSP6dBQWhoUX.png)

```python
print(frame)
frame.unstack()
```



**59.DataFrame 条件查找**

以下面的dataframe为示例：

```python
data = {'animal': ['cat', 'cat', 'snake', 'dog', 'dog', 'cat', 'snake', 'cat', 'dog', 'dog'],
        'age': [2.5, 3, 0.5, np.nan, 5, 2, 4.5, np.nan, 7, 3],
        'visits': [1, 3, 2, 3, 2, 3, 1, 1, 2, 1],
        'priority': ['yes', 'yes', 'no', 'yes', 'no', 'no', 'no', 'yes', 'no', 'no']}

labels = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']
df = pd.DataFrame(data, index=labels)
```



**查找`age`大于3的全部信息：**

```python
df[df['age'] > 3]
```



**根据行列索引切片：**

```python
df.iloc[2:4, 1:3]
```



**60.DataFrame 多重条件查询**

查找 `age<3` 且为 `cat` 的全部数据。

```python
df = pd.DataFrame(data, index=labels)
df[(df['animal'] == 'cat') & (df['age'] < 3)]
```



**61.DataFrame 按关键字查询**

查询动物名为`cat`或`dog`的全部数据。

```python
df3[df3['animal'].isin(['cat', 'dog'])]
```



**62.DataFrame 按标签及列名查询**

选择行索引为`[3, 4, 8]`中的`animal`和`age`属性。

```python
df.loc[df2.index[[3, 4, 8]], ['animal', 'age']]
```



63.



