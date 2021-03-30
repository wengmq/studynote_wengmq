# EXCEL文件

参考：https://www.liujiangblog.com/course/data/234

------

Pandas支持Excle 2003或更高版本文件的读写。在内部，这需要使用附加包xlrd和openpyxl来分别读取XLS和XLSX文件。如果你的环境中没有这两个包，可能需要使用pip或者conda手动安装一下。

使用很简单，看下面的例子：

```
In [104]: xlsx = pd.ExcelFile('d:/ex1.xlsx') # 打开excel文件

In [105]: pd.read_excel(xlsx, 'Sheet1') # 读取指定的表
Out[105]:
   a   b   c   d message
0  1   2   3   4   hello
1  5   6   7   8   world
2  9  10  11  12     foo
```

或者使用更简洁的语法：

```
In [106]: df = pd.read_excel('d:/ex1.xlsx', 'Sheet1')

In [107]: df
Out[107]:
   a   b   c   d message
0  1   2   3   4   hello
1  5   6   7   8   world
2  9  10  11  12     foo
```

将pandas数据写回到excel文件中时，你必须先生成一个ExcelWriter，然后使用to_excel方法将数据写入：

```
In [108]: writer = pd.ExcelWriter('d:/ex2.xlsx') # 生成文件

In [109]: df.to_excel(writer, 'Sheet1') # 写入

In [110]: writer.save()  #关闭文件
```

当然，也可以使用快捷操作：

```
In [112]: df.to_excel('d:/ex3.xlsx')
```