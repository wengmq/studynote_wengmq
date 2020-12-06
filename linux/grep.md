# grep命令

### 简介

grep 是分析每一行信息， 若当中有我们所需要的信息，就将该行拿出来。



### 用法和参数说明

```
[dmtsai@study ~]$ grep [-acinv] [--color=auto] '搜寻字串' filename

选项与参数:

-a :将 binary 文件以 text 文件的方式搜寻数据

-c :计算找到 '搜寻字串' 的次数

-i :忽略大小写的不同，所以大小写视为相同

-n :顺便输出行号

-v :反向选择，亦即显示出没有 '搜寻字串' 内容的那一行!

--color=auto :可以将找到的关键字部分加上颜色的显示喔!
```



### 范例一:将 last 当中，有出现 root 的那一行就取出来;

```
[dmtsai@study ~]$ last | grep 'root'
```



### 范例二:与范例一相反，只要没有 root 的就取出!

```
[dmtsai@study ~]$ last | grep -v 'root'
```



### 范例三:在 last 的输出讯息中，只要有 root 就取出，并且仅取第一栏

```
[dmtsai@study ~]$ last | grep 'root' |cut -d ' ' -f1

# 在取出 root 之后，利用上个指令 cut 的处理，就能够仅取得第一栏啰!
```



### 范例四:取出 /etc/man_db.conf 内含 MANPATH 的那几行

```
[dmtsai@study ~]$ grep --color=auto 'MANPATH' /etc/man_db.conf

....(前面省略)....

MANPATH_MAP /usr/games /usr/share/man
MANPATH_MAP /opt/bin /opt/man
MANPATH_MAP /opt/sbin /opt/man

# 神奇的是，如果加上 --color=auto 的选项，找到的关键字部分会用特殊颜色显示喔!

grep 是个很棒的指令喔!他支持的语法实在是太多了~用在正则表达式里头， 能够处理的数 据实在是多的很~不过，我们这里先不谈正则表达式~下一章再来说明~ 您先了解一下， grep 可以解析一行文字，取得关键字，若该行有存在关键字，就会整行列出来!另外， CentOS 7 当中，默认的 grep 已经主动加上 --color=auto 在 alias 内了喔!
```

