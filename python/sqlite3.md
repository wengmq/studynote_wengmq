# [`sqlite3`](https://docs.python.org/zh-cn/3.8/library/sqlite3.html#module-sqlite3) --- SQLite 数据库 DB-API 2.0 接口模块

**参考：**https://docs.python.org/zh-cn/3.8/library/sqlite3.html

**源代码：** [Lib/sqlite3/](https://github.com/python/cpython/tree/3.8/Lib/sqlite3/)

------

SQLite 是一个C语言库，它可以提供一种轻量级的基于磁盘的数据库，这种数据库不需要独立的服务器进程，也允许需要使用一种非标准的 SQL 查询语言来访问它。一些应用程序可以使用 SQLite 作为内部数据存储。可以用它来创建一个应用程序原型，然后再迁移到更大的数据库，比如 PostgreSQL 或 Oracle。

sqlite3 模块由 Gerhard Häring 编写。它提供了符合 DB-API 2.0 规范的接口，这个规范是 [**PEP 249**](https://www.python.org/dev/peps/pep-0249)。

要使用这个模块，必须先创建一个 [`Connection`](https://docs.python.org/zh-cn/3.8/library/sqlite3.html#sqlite3.Connection) 对象，它代表数据库。下面例子中，数据将存储在 `example.db` 文件中：

```
import sqlite3
con = sqlite3.connect('example.db')
```

你也可以使用 `:memory:` 来创建一个内存中的数据库

当有了 [`Connection`](https://docs.python.org/zh-cn/3.8/library/sqlite3.html#sqlite3.Connection) 对象后，你可以创建一个 [`Cursor`](https://docs.python.org/zh-cn/3.8/library/sqlite3.html#sqlite3.Cursor) 游标对象，然后调用它的 [`execute()`](https://docs.python.org/zh-cn/3.8/library/sqlite3.html#sqlite3.Cursor.execute) 方法来执行 SQL 语句：

```
cur = con.cursor()

# Create table
cur.execute('''CREATE TABLE stocks
               (date text, trans text, symbol text, qty real, price real)''')

# Insert a row of data
cur.execute("INSERT INTO stocks VALUES ('2006-01-05','BUY','RHAT',100,35.14)")

# Save (commit) the changes
con.commit()

# We can also close the connection if we are done with it.
# Just be sure any changes have been committed or they will be lost.
con.close()
```

这些数据被持久化保存了，而且可以在之后的会话中使用它们：

```
import sqlite3
con = sqlite3.connect('example.db')
cur = con.cursor()
```

通常你的 SQL 操作需要使用一些 Python 变量的值。你不应该使用 Python 的字符串操作来创建你的查询语句，因为那样做不安全；它会使你的程序容易受到 SQL 注入攻击（在 https://xkcd.com/327/ 上有一个搞笑的例子，看看有什么后果）

推荐另外一种方法：使用 DB-API 的参数替换。在你的 SQL 语句中，使用 `?` 占位符来代替值，然后把对应的值组成的元组做为 [`execute()`](https://docs.python.org/zh-cn/3.8/library/sqlite3.html#sqlite3.Cursor.execute) 方法的第二个参数。（其他数据库可能会使用不同的占位符，比如 `%s` 或者 `:1`）例如：

```
# Never do this -- insecure!
symbol = 'RHAT'
cur.execute("SELECT * FROM stocks WHERE symbol = '%s'" % symbol)

# Do this instead
t = ('RHAT',)
cur.execute('SELECT * FROM stocks WHERE symbol=?', t)
print(cur.fetchone())

# Larger example that inserts many records at a time
purchases = [('2006-03-28', 'BUY', 'IBM', 1000, 45.00),
             ('2006-04-05', 'BUY', 'MSFT', 1000, 72.00),
             ('2006-04-06', 'SELL', 'IBM', 500, 53.00),
            ]
cur.executemany('INSERT INTO stocks VALUES (?,?,?,?,?)', purchases)
```

要在执行 SELECT 语句后获取数据，你可以把游标作为 [iterator](https://docs.python.org/zh-cn/3.8/glossary.html#term-iterator)，然后调用它的 [`fetchone()`](https://docs.python.org/zh-cn/3.8/library/sqlite3.html#sqlite3.Cursor.fetchone) 方法来获取一条匹配的行，也可以调用 [`fetchall()`](https://docs.python.org/zh-cn/3.8/library/sqlite3.html#sqlite3.Cursor.fetchall) 来得到包含多个匹配行的列表。

下面是一个使用迭代器形式的例子：

\>>>

```
>>> for row in cur.execute('SELECT * FROM stocks ORDER BY price'):
        print(row)

('2006-01-05', 'BUY', 'RHAT', 100, 35.14)
('2006-03-28', 'BUY', 'IBM', 1000, 45.0)
('2006-04-06', 'SELL', 'IBM', 500, 53.0)
('2006-04-05', 'BUY', 'MSFT', 1000, 72.0)
```

参见

- [https://www.sqlite.org](https://www.sqlite.org/)

  SQLite的主页；它的文档详细描述了它所支持的 SQL 方言的语法和可用的数据类型。

- https://www.w3schools.com/sql/

  学习 SQL 语法的教程、参考和例子。

- [**PEP 249**](https://www.python.org/dev/peps/pep-0249) - DB-API 2.0 规范

  PEP 由 Marc-André Lemburg 撰写。



