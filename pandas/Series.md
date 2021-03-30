# Series

参考：https://www.liujiangblog.com/course/data/219

------

(以下内容部分参考《Python数据分析》)

Pandas的核心是三大数据结构：Series、DataFrame和Index。绝大多数操作都是围绕这三种结构进行的。

Series是一个一维的数组对象，它包含一个值序列和一个对应的索引序列。 Numpy的一维数组通过隐式定义的整数索引获取元素值，而Series用一种显式定义的索引与元素关联。显式索引让Series对象拥有更强的能力，索引也不再仅仅是整数，还可以是别的类型，比如字符串，索引也不需要连续，也可以重复，自由度非常高。

最基本的生成方式是使用Series构造器：

```
In [1]: import pandas as pd

In [4]: s = pd.Series([7,-3,4,-2])

In [5]: s
Out[5]:
0    7
1   -3
2    4
3   -2
dtype: int64
```

打印的时候，自动对齐了，看起来比较美观。左边是索引，右边是实际对应的值。默认的索引是0到N-1（N是数据的长度）。可以通过values和index属性分别获取Series对象的值和索引：

```
In [5]: s.dtype
Out[5]: dtype('int64')

In [6]: s.values
Out[6]: array([ 7, -3,  4, -2], dtype=int64)

In [7]: s.index
Out[7]: RangeIndex(start=0, stop=4, step=1)
```

可以在创建Series对象的时候指定索引：

```
In [8]: s2 = pd.Series([7,-3,4,-2], index=['d','b','a','c'])

In [9]: s2
Out[9]:
d    7
b   -3
a    4
c   -2
dtype: int64

In [10]: s2.index
Out[10]: Index(['d', 'b', 'a', 'c'], dtype='object')

In [4]: pd.Series(5, index=list('abcde'))
Out[4]:
a    5
b    5
c    5
d    5
e    5
dtype: int64

In [5]: pd.Series({2:'a',1:'b',3:'c'}, index=[3,2]) # 通过index筛选结果
Out[5]:
3    c
2    a
dtype: object
```

也可以在后期，直接修改index：

```
In [33]: s
Out[33]:
0    7
1   -3
2    4
3   -2
dtype: int64

In [34]: s.index = ['a','b','c','d']

In [35]: s
Out[35]:
a    7
b   -3
c    4
d   -2
dtype: int64
```

类似Python的列表和Numpy的数组，Series也可以通过索引获取对应的值：

```
In [11]: s2['a']
Out[11]: 4

In [12]: s2[['c','a','d']]
Out[12]:
c   -2
a    4
d    7
dtype: int64
```

也可以对Seires执行一些类似Numpy的通用函数操作：

```
In [13]: s2[s2>0]
Out[13]:
d    7
a    4
dtype: int64

In [14]: s2*2
Out[14]:
d    14
b    -6
a     8
c    -4
dtype: int64

In [15]: import numpy as np

In [16]: np.exp(s2)
Out[16]:
d    1096.633158
b       0.049787
a      54.598150
c       0.135335
dtype: float64
```

因为索引可以是字符串，所以从某个角度看，Series又比较类似Python的有序字典，所以可以使用in操作：

```
In [17]: 'b' in s2
Out[17]: True

In [18]: 'e'in s2
Out[18]: False
```

自然，我们也会想到使用Python的字典来创建Series：

```
In [19]: dic = {'beijing':35000,'shanghai':71000,'guangzhou':16000,'shenzhen':5000}

In [20]: s3=pd.Series(dic)

In [21]: s3
Out[21]:
beijing      35000
shanghai     71000
guangzhou    16000
shenzhen     5000
dtype: int64

In [14]: s3.keys() # 自然，具有类似字典的方法
Out[14]: Index(['beijing', 'shanghai', 'guangzhou', 'shenzhen'], dtype='object')

In [15]: s3.items()
Out[15]: <zip at 0x1a5c2d88c88>

In [16]: list(s3.items())
Out[16]:
[('beijing', 35000),
 ('shanghai', 71000),
 ('guangzhou', 16000),
 ('shenzhen', 5000)]

In [18]: s3['changsha'] = 20300
```

我们看下面的例子：

```
In [22]: city = ['nanjing', 'shanghai','guangzhou','beijing']

In [23]: s4=pd.Series(dic, index=city)

In [24]: s4
Out[24]:
nanjing          NaN
shanghai     71000.0
guangzhou    16000.0
beijing      35000.0
dtype: float64
```

city列表中，多了‘nanjing’，但少了‘shenzhen’。Pandas会依据city中的关键字去dic中查找对应的值，因为dic中没有‘nanjing’，这个值缺失，所以以专门的标记值NaN表示。因为city中没有‘shenzhen’，所以在s4中也不会存在‘shenzhen’这个条目。可以看出，索引很关键，在这里起到了决定性的作用。

在Pandas中，可以使用isnull和notnull函数来检查缺失的数据：

```
In [25]: pd.isnull(s4)
Out[25]:
nanjing       True
shanghai     False
guangzhou    False
beijing      False
dtype: bool

In [26]: pd.notnull(s4)
Out[26]:
nanjing      False
shanghai      True
guangzhou     True
beijing       True
dtype: bool

In [27]: s4.isnull()
Out[27]:
nanjing       True
shanghai     False
guangzhou    False
beijing      False
dtype: bool
```

可以为Series对象和其索引设置name属性，这有助于标记识别：

```
In [29]: s4.name = 'people'

In [30]: s4.index.name= 'city'

In [31]: s4
Out[31]:
city
nanjing          NaN
shanghai     71000.0
guangzhou    16000.0
beijing      35000.0
Name: people, dtype: float64

In [32]: s4.index
Out[32]: Index(['nanjing', 'shanghai', 'guangzhou', 'beijing'], dtype='object', name='city')
```