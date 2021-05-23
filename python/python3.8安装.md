# Linux安装Python3.8.1的教程详解

 更新时间：2020年02月09日 10:46:39  作者：四海飞鹰  

这篇文章主要介绍了Linux安装Python3.8.1的教程，本文以linux安装python3.8版本为例给大家详细说明，感兴趣的朋友跟随小编一起看看吧

本例以Linux上安装Pyhton3.8版本为例进行说明

1、依赖包安装

```
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel libffi-devel
```

2、下载包：

https://www.python.org/ftp/python/3.8.1/

wget https://www.python.org/ftp/python/3.8.1/Python-3.8.1.tgz

3、解压：

```
tar -zxvf Python-3.8.1.tgz
```

4、安装：

```
cd Python-3.8.1``./configure --prefix=/usr/local/python3``make && make install
```

5、建立软连接

```
ln -s /usr/local/python3/bin/python3.8 /usr/bin/python3``ln -s /usr/local/python3/bin/pip3.8 /usr/bin/pip3
```

6、验证是否安装成功

执行python3命令

![img](https://img.jbzj.com/file_images/article/202002/202029104414885.jpg?202019104528)

执行pip3命令

![img](https://img.jbzj.com/file_images/article/202002/202029104414886.jpg?202019104528)

如果执行上面命令均得到如图结果，则说明安装成功。