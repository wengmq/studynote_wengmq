#  jupyter搭建

参考：https://blog.csdn.net/budong282712018/article/details/103053314

### 环境设置

```
# 安装virtualenvwrapper
sudo pip install virtualenvwrapper

# 设置virtualenvwrapper
mkdir ~/python/env 
vim ~/.bashrc

# 加入一下两句句
export WORKON_HOME=~/python/env
source /usr/bin/virtualenvwrapper.sh

# 刷新
source ~/.bashrc

# 创建虚拟环境
mkvittualenv jupyter -p `which python3`


```



### jupyter安装和启动

```
# 进入虚拟环境
workon jupyter

1. 安装ipython, jupyter 

pip install ipython  pip install jupyter

 
2. 生成配置文件
$ jupyter notebook --generate-config
Writing default config to: /Users/wengmingqiang/.jupyter/jupyter_notebook_config.py

#/root/.jupyter/jupyter_notebook_config.py


3. 生成密码：990124

root@50eb5057baac /]# ipython  Python 3.5.1 (default, Oct 21 2016, 21:37:19)  

Type 'copyright', 'credits' or 'license' for more information  IPython 6.2.1 -- An enhanced Interactive Python. Type '?' for help.   

In [1]: from notebook.auth import passwd

In [2]: passwd()
Enter password:
Verify password:
Out[2]: 'argon2:$argon2id$v=19$m=10240,t=10,p=8$kZZtDJ3VoMO/Zzo41vyGTQ$XbqwFe/Yc93l2x++C2m2fg'

 
 

3. 修改默认配置文件

vim /Users/wengmingqiang/.jupyter/jupyter_notebook_config.py


添加以下内容：
c.NotebookApp.ip='*' #设置访问notebook的ip，*表示所有IP，这里设置ip为都可访问  
c.NotebookApp.password = u'argon2:$argon2id$v=19$m=10240,t=10,p=8$kZZtDJ3VoMO/Zzo41vyGTQ$XbqwFe/Yc93l2x++C2m2fg' #填写刚刚生成的密文  
c.NotebookApp.open_browser = False # 禁止notebook启动时自动打开浏览器
c.NotebookApp.port =8889 #指定访问的端口，默认是8888。 
c.NotebookApp.notebook_dir = u'/Users/wengmingqiang/python/BSTC/bstc/jupyter/tools/ ' # >即设置jupyter启动后默认的根目录

 
4.    启动


[plain] view plain copy <code class="language-plain">[root@346086094cbe /]# jupyter notebook --allow-root    [W 17:17:04.106 NotebookApp] WARNING: The notebook server is listening on all IP addresses and not using encryption. This is not recommended.    [I 17:17:04.111 NotebookApp] Serving notebooks from local directory: /    [I 17:17:04.112 NotebookApp] 0 active kernels    [I 17:17:04.112 NotebookApp] The Jupyter Notebook is running at:    [I 17:17:04.112 NotebookApp] http://[all ip addresses on your system]:8889/    [I 17:17:04.112 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).  </code> 



5. 然后你就可以在浏览器里敲入你的地址 http://yourip:8889/， 看到如下界面。


安装成功

6. 启动Jupyter的开发窗口，点击右上角的new
```

