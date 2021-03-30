# DataFrame

参考：https://www.liujiangblog.com/course/data/220

------

DataFrame是Pandas的核心数据结构，表示的是二维的矩阵数据表，类似关系型数据库的结构，每一列可以是不同的值类型，比如数值、字符串、布尔值等等。DataFrame既有行索引，也有列索引，它可以被看做为一个共享相同索引的Series的字典。

创建DataFrame对象的方法有很多，最常用的是利用包含等长度列表或Numpy数组的字典来生成。可以查看DataFrame对象的columns和index属性。

```
In [37]: data = {'state':['beijing','beijing','beijing','shanghai','shanghai','shanghai'],
    ...: 'year':[2000,2001,2002,2001,2002,2003],
    ...: 'pop':[1.5, 1.7,3.6,2.4,2.9,3.2
    ...: ]}

In [38]: f = pd.DataFrame(data)

In [39]: f
Out[39]:
      state  year  pop
0   beijing  2000  1.5
1   beijing  2001  1.7
2   beijing  2002  3.6
3  shanghai  2001  2.4
4  shanghai  2002  2.9
5  shanghai  2003  3.2

In [61]: f.columns
Out[61]: Index(['state', 'year', 'pop'], dtype='object')

In [62]: f.index
Out[62]: RangeIndex(start=0, stop=6, step=1)

In [10]: f.dtypes
Out[10]:
state     object
year       int64
pop      float64
dtype: object

In [11]: f.values  # 按行查看
Out[11]:
array([['beijing', 2000, 1.5],
       ['beijing', 2001, 1.7],
       ['beijing', 2002, 3.6],
       ['shanghai', 2001, 2.4],
       ['shanghai', 2002, 2.9],
       ['shanghai', 2003, 3.2]], dtype=object)
```

上面自动生成了0-5的索引。

可以使用head方法查看DataFrame对象的前5行，用tail方法查看后5行。或者head(3)，tail(3)指定查看行数：

```
In [40]: f.head()
Out[40]:
      state  year  pop
0   beijing  2000  1.5
1   beijing  2001  1.7
2   beijing  2002  3.6
3  shanghai  2001  2.4
4  shanghai  2002  2.9

In [41]: f.tail()
Out[41]:
      state  year  pop
1   beijing  2001  1.7
2   beijing  2002  3.6
3  shanghai  2001  2.4
4  shanghai  2002  2.9
5  shanghai  2003  3.2
```

DataFrame对象中的state/year/pop，其实就是列索引，可以在创建的时候使用参数名columns指定它们的先后顺序：

```
In [44]: pd.DataFrame(data, columns=['year','state','pop'])
Out[44]:
   year     state  pop
0  2000   beijing  1.5
1  2001   beijing  1.7
2  2002   beijing  3.6
3  2001  shanghai  2.4
4  2002  shanghai  2.9
5  2003  shanghai  3.2
```

当然，也可以使用参数名index指定行索引：

```
In [45]: f2 = pd.DataFrame(data, columns=['year','state','pop'],index=['a','b','c','d','e','f'])

In [47]: f2
Out[47]:
   year     state  pop
a  2000   beijing  1.5
b  2001   beijing  1.7
c  2002   beijing  3.6
d  2001  shanghai  2.4
e  2002  shanghai  2.9
f  2003  shanghai  3.2
```

可以使用columns列索引来检索一列：

```
In [49]: f2['year']
Out[49]:
a    2000
b    2001
c    2002
d    2001
e    2002
f    2003
Name: year, dtype: int64

In [52]: f2.state  # 属性的形式来检索。这种方法bug多，比如属性名不是纯字符串，或者与其它方法同名
Out[52]:
a     beijing
b     beijing
c     beijing
d    shanghai
e    shanghai
f    shanghai
Name: state, dtype: object
```

但是检索一行却不能通过f2['a']这种方式，而是需要通过loc方法进行选取：

```
In [53]: f2.loc['a']
Out[53]:
year        2000
state    beijing
pop          1.5
Name: a, dtype: object
```

当然，可以给DataFrame对象追加列：

```
In [54]: f2['debt'] = 12

In [55]: f2
Out[55]:
   year     state  pop  debt
a  2000   beijing  1.5    12
b  2001   beijing  1.7    12
c  2002   beijing  3.6    12
d  2001  shanghai  2.4    12
e  2002  shanghai  2.9    12
f  2003  shanghai  3.2    12

In [56]: f2['debt'] = np.arange(1,7)

In [57]: f2
Out[57]:
   year     state  pop  debt
a  2000   beijing  1.5     1
b  2001   beijing  1.7     2
c  2002   beijing  3.6     3
d  2001  shanghai  2.4     4
e  2002  shanghai  2.9     5
f  2003  shanghai  3.2     6

In [58]: val = pd.Series([1,2,3],index = ['c','d','f'])

In [59]: f2['debt'] = val

In [60]: f2  # 缺失值以NaN填补
Out[60]:
   year     state  pop  debt
a  2000   beijing  1.5   NaN
b  2001   beijing  1.7   NaN
c  2002   beijing  3.6   1.0
d  2001  shanghai  2.4   2.0
e  2002  shanghai  2.9   NaN
f  2003  shanghai  3.2   3.0
```

那么如何给DataFrame追加行呢？

```
>>> data = {'state':['beijing','beijing','beijing','shanghai','shanghai','shanghai'],
    ...: 'year':[2000,2001,2002,2001,2002,2003],
    ...: 'pop':[1.5, 1.7,3.6,2.4,2.9,3.2
    ...: ]}
>>> df = pd.DataFrame(data,index=list('abcdef'))
>>> df

    state   year    pop
a   beijing 2000    1.5
b   beijing 2001    1.7
c   beijing 2002    3.6
d   shanghai    2001    2.4
e   shanghai    2002    2.9
f   shanghai    2003    3.2

>>> df1 = df.loc['a']
>>> df1

state    beijing
year        2000
pop          1.5
Name: a, dtype: object

>>> df.append(df1)

state   year    pop
a   beijing 2000    1.5
b   beijing 2001    1.7
c   beijing 2002    3.6
d   shanghai    2001    2.4
e   shanghai    2002    2.9
f   shanghai    2003    3.2
a   beijing 2000    1.5
```

可以使用del方法删除指定的列：

```
In [63]: f2['new'] = f2.state=='beijing'

In [64]: f2
Out[64]:
   year     state  pop  debt    new
a  2000   beijing  1.5   NaN   True
b  2001   beijing  1.7   NaN   True
c  2002   beijing  3.6   1.0   True
d  2001  shanghai  2.4   2.0  False
e  2002  shanghai  2.9   NaN  False
f  2003  shanghai  3.2   3.0  False

In [65]: del f2.new   # 要注意的是我们有时候不能这么调用f2的某个列，并执行某个操作。这是个坑。
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-65-03e4ec812cdb> in <module>()
----> 1 del f2.new

AttributeError: new

In [66]: del f2['new']

In [67]: f2.columns
Out[67]: Index(['year', 'state', 'pop', 'debt'], dtype='object')
```

需要注意的是：从DataFrame中选取的列是数据的视图，而不是拷贝。因此，对选取列的修改会反映到DataFrame上。如果需要复制，应当使用copy方法。

可以使用类似Numpy的T属性，将DataFrame进行转置：

```
In [68]: f2.T
Out[68]:
             a        b        c         d         e         f
year      2000     2001     2002      2001      2002      2003
state  beijing  beijing  beijing  shanghai  shanghai  shanghai
pop        1.5      1.7      3.6       2.4       2.9       3.2
debt       NaN      NaN        1         2       NaN         3
```

DataFrame对象同样具有列名、索引名，也可以查看values：

```
In [70]: f2.index.name = 'order';f2.columns.name='key'

In [71]: f2
Out[71]:
key    year     state  pop  debt
order
a      2000   beijing  1.5   NaN
b      2001   beijing  1.7   NaN
c      2002   beijing  3.6   1.0
d      2001  shanghai  2.4   2.0
e      2002  shanghai  2.9   NaN
f      2003  shanghai  3.2   3.0

In [72]: f2.values
Out[72]:
array([[2000, 'beijing', 1.5, nan],
       [2001, 'beijing', 1.7, nan],
       [2002, 'beijing', 3.6, 1.0],
       [2001, 'shanghai', 2.4, 2.0],
       [2002, 'shanghai', 2.9, nan],
       [2003, 'shanghai', 3.2, 3.0]], dtype=object)
```

最后，DataFrame有一个Series所不具备的方法，那就是info！通过这个方法，可以看到DataFrame的一些整体信息情况：

```
In [73]: f.info()
Out[73]:
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 6 entries, 0 to 5
Data columns (total 3 columns):
state    6 non-null object
year     6 non-null int64
pop      6 non-null float64
dtypes: float64(1), int64(1), object(1)
memory usage: 224.0+ bytes
```