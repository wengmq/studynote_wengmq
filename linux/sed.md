# sed命令

### 简介

sed 本身也是一个管线命令，可以分析 standard input 的啦! 而 且 sed 还可以将数据进行取代、删除、新增、撷取特定行等等的功能呢!



### 用法和参数说明

```
[dmtsai@study ~]$ sed [-nefr] [动作]
  
  选项与参数:
    -n :使用安静(silent)模式。在一般 sed 的用法中，所有来自 STDIN 的数据一般都会被列出到屏幕上。
    但如果加上 -n 参数后，则只有经过 sed 特殊处理的那一行(或者动作)才会被列出来。
    -e :直接在命令行界面上进行 sed 的动作编辑;
    -f :直接将 sed 的动作写在一个文件内， -f filename 则可以执行 filename 内的 sed 动作;
    -r :sed 的动作支持的是延伸型正则表达式的语法。(默认是基础正则表达式语法)
    -i :直接修改读取的文件内容，而不是由屏幕输出。
  
  动作说明: [n1[,n2]]function
    n1, n2 :不见得会存在，一般代表“选择进行动作的行数”，举例来说，如果我的动作
    是需要在 10 到 20 行之间进行的，则“ 10,20[动作行为] ”
    function 有下面这些咚咚:
    a :新增， a 的后面可以接字串，而这些字串会在新的一行出现(目前的下一行)~
    c :取代， c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行!
    d :删除，因为是删除啊，所以 d 后面通常不接任何咚咚;
    i :插入， i 的后面可以接字串，而这些字串会在新的一行出现(目前的上一行);
    p :打印，亦即将某个选择的数据印出。通常 p 会与参数 sed -n 一起运行~
    s :取代，可以直接进行取代的工作哩!通常这个 s 的动作可以搭配正则表达式!
    例如 1,20s/old/new/g 就是啦!
```



### 范例一:将 /etc/passwd 的内容列出并且打印行号，同时，请将第 2~5 行删除!

> ```
> [dmtsai@study ~]$ nl /etc/passwd | sed '2,5d'
> ```

```
1  root:x:0:0:root:/root:/bin/bash
6  sync:x:5:0:sync:/sbin:/bin/sync
7  shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
.....(后面省略).....
```

看到了吧?sed 的动作为 '2,5d' ，那个 d 就是删除!因为 2-5 行给他删除了，所以显示的数据 就没有 2-5 行啰~ 另外，注意一下，原本应该是要下达 sed -e 才对，没有 -e 也行啦!同时 也要注意的是， sed 后面接的动作，请务必以 '' 两个单引号括住喔!

如果题型变化一下，举例来说，如果只要删除第 2 行，可以使用“ nl /etc/passwd | sed '2d' ”来 达成， 至于若是要删除第 3 到最后一行，则是“ nl /etc/passwd | sed '3,$d' ”的啦，那个钱字 号“ $ ”代表最后一行!



### 范例二:承上题，在第二行后(亦即是加在第三行)加上“drink tea?”字样!

[dmtsai@study ~]$ nl /etc/passwd | sed '2a drink tea'

```
  1  root:x:0:0:root:/root:/bin/bash
  2  bin:x:1:1:bin:/bin:/sbin/nologin
drink tea
	3  daemon:x:2:2:daemon:/sbin:/sbin/nologi
.....(后面省略).....
```

嘿嘿!在 a 后面加上的字串就已将出现在第二行后面啰!那如果是要在第二行前呢?“ nl /etc/passwd | sed '2i drink tea' ”就对啦!就是将“ a ”变成“ i ”即可。 增加一行很简单，那如果 是要增将两行以上呢?



### 范例三:在第二行后面加入两行字，例如“Drink tea or .....”与“drink beer?”

> ```
> [dmtsai@study ~]$ nl /etc/passwd | sed '2a Drink tea or ......\ 
> > drink beer ?'
> ```

```
  1  root:x:0:0:root:/root:/bin/bash
  2  bin:x:1:1:bin:/bin:/sbin/nologin
Drink tea or ......
drink beer ?
 3  daemon:x:2:2:daemon:/sbin:/sbin/nologin
 .....(后面省略).....
```



这个范例的重点是“我们可以新增不只一行喔!可以新增好几行”但是每一行之间都必须要以反 斜线“ \ ”来进行新行的增加喔!所以，上面的例子中，我们可以发现在第一行的最后面就有 \ 存在啦!在多行新增的情况下， \ 是一定要的喔!			 	



### 范例四:我想将第2-5行的内容取代成为“No 2-5 number”呢?

```
 [dmtsai@study ~]$ nl /etc/passwd | sed '2,5c No 2-5 number'
 1  root:x:0:0:root:/root:/bin/bash
 No 2-5 number
 6  sync:x:5:0:sync:/sbin:/bin/sync
```

通过这个方法我们就能够将数据整行取代了!非常容易吧!sed 还有更好用的东东!我们以前 想要列出第 11~20 行， 得要通过“head -n 20 | tail -n 10”之类的方法来处理，很麻烦啦~ sed 则可以简单的直接取出你想要的那几行!是通过行号来捉的喔!看看下面的范例先:



### 范例五:仅列出 /etc/passwd 文件内的第 5-7 行

```
[dmtsai@study ~]$ nl /etc/passwd | sed -n '5,7p'
5  lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
6  sync:x:5:0:sync:/sbin:/bin/sync
7  shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
```

上述的指令中有个重要的选项“ -n ”，按照说明文档，这个 -n 代表的是“安静模式”! 那么为什 么要使用安静模式呢?你可以自行下达 sed '5,7p' 就知道了 (5-7 行会重复输出)! 有没有 加上 -n 的参数时，输出的数据可是差很多的喔!你可以通过这个 sed 的以行为单位的显示功 能， 就能够将某一个文件内的某些行号捉出来查阅!很棒的功能!不是吗?



### 部分数据的搜寻并取代的功能

 除了整行的处理模式之外， sed 还可以用行为单位进行部分数据的搜寻并取代的功能喔! 基

本上 sed 的搜寻与取代的与 vi 相当的类似!他有点像这样: 

```
sed 's/要被取代的字串/新的字串/g'
```

上表中特殊字体的部分为关键字，请记下来!至于三个斜线分成两栏就是新旧字串的替换 啦! 我们使用下面这个取得 IP 数据的范例，一段一段的来处理给您瞧瞧，让你了解一下什么 是咱们所谓的搜寻并取代吧!

```
步骤一:先观察原始讯息，利用 /sbin/ifconfig 查询 IP 为何? 
    [dmtsai@study ~]$ /sbin/ifconfig eth0
    eth0: flags=4163&lt;UP,BROADCAST,RUNNING,MULTICAST&gt;  mtu 1500
    inet 192.168.1.100 netmask 255.255.255.0 broadcast 192.168.1.255
    inet6 fe80::5054:ff:fedf:e174  prefixlen 64  scopeid 0x20&lt;link&gt;
    ether 52:54:00:df:e1:74 txqueuelen 1000 (Ethernet)
    .....(以下省略).....
# 因为我们还没有讲到 IP ，这里你先有个概念即可啊!我们的重点在第二行，
# 也就是 192.168.1.100 那一行而已!先利用关键字捉出那一行!

步骤二:利用关键字配合 grep 撷取出关键的一行数据
    [dmtsai@study ~]$ /sbin/ifconfig eth0 | grep 'inet '
    inet 192.168.1.100 netmask 255.255.255.0 broadcast 192.168.1.255
    # 当场仅剩下一行!要注意， CentOS 7 与 CentOS 6 以前的 ifconfig 指令输出结果不太相同，
    # 鸟哥这个范例主要是针对 CentOS 7 以后的喔!接下来，我们要将开始到 addr: 通通删除，
    # 就是像下面这样:
    # inet 192.168.1.100 netmask 255.255.255.0 broadcast 192.168.1.255
    # 上面的删除关键在于“ ^.*inet ”啦!正则表达式出现! ^_^

步骤三:将 IP 前面的部分予以删除
      [dmtsai@study ~]$ /sbin/ifconfig eth0 | grep 'inet ' | sed 's/^.*inet //g'
     192.168.1.100 netmask 255.255.255.0 broadcast 192.168.1.255
    # 仔细与上个步骤比较一下，前面的部分不见了!接下来则是删除后续的部分，亦即:
      192.168.1.100 netmask 255.255.255.0 broadcast 192.168.1.255
    # 此时所需的正则表达式为:“ ' *netmask.*$ ”就是啦!

步骤四:将 IP 后面的部分予以删除
      [dmtsai@study ~]$ /sbin/ifconfig eth0 | grep 'inet ' | sed 's/^.*inet //g' | sed 's/ *netmask.*$//g'
     192.168.1.100
```



通过这个范例的练习也建议您依据此一步骤来研究你的指令!就是先观察，然后再一层一层 的试做， 如果有做不对的地方，就先予以修改，改完之后测试，成功后再往下继续测试。以 鸟哥上面的介绍中， 那一大串指令就做了四个步骤!对吧! ^_^

让我们再来继续研究 sed 与正则表达式的配合练习!假设我只要 MAN 存在的那几行数据， 但是含有 # 在内的注解我不想要，而且空白行我也不要!此时该如何处理呢?可以通过这几 个步骤来实作看看:

```
步骤一:先使用 grep 将关键字 MAN 所在行取出来
     [dmtsai@study ~]$ cat /etc/man_db.conf | grep 'MAN'
   # MANDATORY_MANPATH manpath_element
   # MANPATH_MAP path_element manpath_element
   # MANDB_MAP global_manpath [relative_catpath]
   # every automatically generated MANPATH includes these fields
	 ....(后面省略)....

步骤二:删除掉注解之后的数据!
    [dmtsai@study ~]$ cat /etc/man_db.conf | grep 'MAN'| sed 's/#.*$//g'
   MANDATORY_MANPATH /usr/man
  ....(后面省略)....
   # 从上面可以看出来，原本注解的数据都变成空白行啦!所以，接下来要删除掉空白行
    [dmtsai@study ~]$ cat /etc/man_db.conf | grep 'MAN'| sed 's/#.*$//g' | sed '/^$/d'
   MANDATORY_MANPATH /usr/man
   MANDATORY_MANPATH /usr/share/man
   MANDATORY_MANPATH /usr/local/share/man
  ....(后面省略)....
```

 

### 直接修改文件内容(危险动作)

你以为 sed 只有这样的能耐吗?那可不! sed 甚至可以直接修改文件的内容呢!而不必使用 管线命令或数据流重导向! 不过，由于这个动作会直接修改到原始的文件，所以请你千万不 要随便拿系统配置文件来测试喔! 我们还是使用你下载的 regular_express.txt 文件来测试看 看吧!

##### 范例六:利用 sed 将 regular_express.txt 内每一行结尾若为 . 则换成 !

```
[dmtsai@study ~]$ sed -i 's/\.$/\!/g' regular_express.txt

# 上头的 -i 选项可以让你的 sed 直接去修改后面接的文件内容而不是由屏幕输出喔!
# 这个范例是用在取代!请您自行 cat 该文件去查阅结果啰!

```



##### 范例七:利用 sed 直接在 regular_express.txt 最后一行加入“# This is a test”

```
[dmtsai@study ~]$ sed -i '$a # This is a test' regular_express.txt

# 由于 $ 代表的是最后一行，而 a 的动作是新增，因此该文件最后新增啰!
```



sed 的“ -i ”选项可以直接修改文件内容，这功能非常有帮助!举例来说，如果你有一个 100 万 行的文件，你要在第 100 行加某些文字，此时使用 vim 可能会疯掉!因为文件太大了!那怎 办?就利用 sed 啊!通过 sed 直接修改/取代的功能，你甚至不需要使用 vim 去修订!很棒 吧!

总之，这个 sed 不错用啦!而且很多的 shell script 都会使用到这个指令的功能~ sed 可以帮 助系统管理员管理好日常的工作喔!要仔细的学习呢!



##### 范例八: 删除指定内容的行

1.以删除文件example.txt中包含字符串"=yes"的行为例,example.txt文件有以下内容:

dadasdfsadf=yes=sds

dsdadasdkfk

dsdsdds=sye

kgfjdfdf=yes==-

 

2.准备删除:

sed -i '/=yes/d' example.txt

 

3.删除后，example.txt的内容如下:

dsdadasdkfk
dsdsdds=sye