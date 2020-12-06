# cut命令

### 简介

什么是撷取命令啊?说穿了，就是将一段数据经过分析后，取出我们所想要的。或者是经由 分析关键字，取得我们所想要的那一行! 不过，要注意的是，一般来说，撷取讯息通常是针 对“一行一行”来分析的。

cut 不就是“切”吗?没错啦!这个撷取指令可以将一段讯息的某一段给他“切”出来~ 处理的讯息是以“行”为单位!



###  用法和参数说明

```
[dmtsai@study ~]$ cut -d '分隔字符' -f <==用于有特定分隔字符
[dmtsai@study ~]$ cut -c 字符区间 <==用于排列整齐的讯息
 
 选项与参数:
 -d :后面接分隔字符。与 -f 一起使用;
 -f :依据 -d 的分隔字符将一段讯息分区成为数段，用 -f 取出第几段的意思;
 -c :以字符 (characters) 的单位取出固定字符区间;
 
```



### 范例一:将 PATH 变量取出，我要找出第五个路径。

```
[dmtsai@study ~]$ echo ${PATH}
/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/dmtsai/.local/bin:/home/dmtsai/bin
#      1      |    2   |       3       |    4    |           5

[dmtsai@study ~]$ echo ${PATH} | cut -d ':' -f 5
# 如同上面的数字显示，我们是以“ : ”作为分隔，因此会出现 /home/dmtsai/.local/bin

# 那么如果想要列出第 3 与第 5 呢?，就是这样:
[dmtsai@study ~]$ echo ${PATH} | cut -d ':' -f 3,5
```



### 范例二:将 export 输出的讯息，取得第 12 字符以后的所有字串

```
[dmtsai@study ~]$ export
declare -x HISTCONTROL="ignoredups"
declare -x HISTSIZE="1000"
declare -x HOME="/home/dmtsai"
declare -x HOSTNAME="study.centos.vbird"
.....(其他省略).....

# 注意看，每个数据都是排列整齐的输出!如果我们不想要“ declare -x ”时，就得这么做:
[dmtsai@study ~]$ export | cut -c 12-
HISTCONTROL="ignoredups"
HISTSIZE="1000"
HOME="/home/dmtsai"
HOSTNAME="study.centos.vbird"
.....(其他省略).....

# 知道怎么回事了吧?用 -c 可以处理比较具有格式的输出数据!
# 我们还可以指定某个范围的值，例如第 12-20 的字符，就是 cut -c 12-20 等等!
```



### 范例三:用 last 将显示的登陆者的信息中，仅留下使用者大名

```
[dmtsai@study ~]$ last
root pts/1 192.168.201.101 Sat Feb 7 12:35 still logged in
root pts/1 192.168.201.101 Fri Feb 6 12:13 - 18:46 (06:33)
root pts/1 192.168.201.254 Thu Feb 5 22:37 - 23:53 (01:16)
# last 可以输出“帐号/终端机/来源/日期时间”的数据，并且是排列整齐的
[dmtsai@study ~]$ last | cut -d ' ' -f 1
# 由输出的结果我们可以发现第一个空白分隔的字段代表帐号，所以使用如上指令:
# 但是因为 root pts/1 之间空格有好几个，并非仅有一个，所以，如果要找出
# pts/1 其实不能以 cut -d ' ' -f 1,2 喔!输出的结果会不是我们想要的。
```

