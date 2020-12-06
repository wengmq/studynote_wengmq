# sort命令

### 简介

sort 是很有趣的指令，他可以帮我们进行排序，而且可以依据不同的数据型态来排序喔! 例 如数字与文字的排序就不一样。此外，排序的字符与语系的编码有关，因此， 如果您需要排 序时，建议使用 LANG=C 来让语系统一，数据排序比较好一些。



### 用法和参数说明

```
[dmtsai@study ~]$ sort [-fbMnrtuk] [file or stdin]

选项与参数:

-f :忽略大小写的差异，例如 A 与 a 视为编码相同;

-b :忽略最前面的空白字符部分;

-M :以月份的名字来排序，例如 JAN, DEC 等等的排序方法;

-n :使用“纯数字”进行排序(默认是以文字体态来排序的);

-r :反向排序;

-u :就是 uniq ，相同的数据中，仅出现一行代表;

-t :分隔符号，默认是用 [tab] 键来分隔;

-k :以那个区间 (field) 来进行排序的意思
```



### 范例一:个人帐号都记录在 /etc/passwd 下，请将帐号进行排序。

[dmtsai@study ~]$ cat /etc/passwd | sort

```
abrt:x:173:173::/etc/abrt:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
alex:x:1001:1002::/home/alex:/bin/bash
```

\# 这里省略很多的输出~由上面的数据看起来， sort 是默认“以第一个”数据来排序，

\# 而且默认是以“文字”型态来排序的喔!所以由 a 开始排到最后啰!



### 范例二:/etc/passwd 内容是以 : 来分隔的，我想以第三栏来排序，该如何?

[dmtsai@study ~]$ cat /etc/passwd | sort -t ':' -k 3

```
root:x:0:0:root:/root:/bin/bash
dmtsai:x:1000:1000:dmtsai:/home/dmtsai:/bin/bash
alex:x:1001:1002::/home/alex:/bin/bash
arod:x:1002:1003::/home/arod:/bin/bash
```

\# 看到特殊字体的输出部分了吧?怎么会这样排列啊?呵呵!没错啦~

\# 如果是以文字体态来排序的话，原本就会是这样，想要使用数字排序:

\# cat /etc/passwd | sort -t ':' -k 3 -n

\# 这样才行啊!用那个 -n 来告知 sort 以数字来排序啊!



### 范例三:利用 last ，将输出的数据仅取帐号，并加以排序 

[dmtsai@study ~]$ last | cut -d ' ' -f1 | sort