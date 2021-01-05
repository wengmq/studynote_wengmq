

# virtualenvwrapper

官网地址：https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html



## 安装virtualenvwrapper

```
pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple virtualenvwrapper
```



## 安装成功后，设置开机启动

vim ~/.zshrc （看你使用哪个shell） 添加以下语句

```
export WORKON_HOME=$HOME/.virtualenvs  # 设置virtualenv的统一管理目录
source /usr/local/bin/virtualenvwrapper.sh 
```



## 创建虚拟环境

-p 可以指定python解释器的位置， env_django_1可以替换成你要创建的虚拟环境的名字

```
mkvirtualenv env_django_1 -p `which python3`
```



## 退出虚拟环境

```
deactivate
```



## 查看有哪些虚拟环境

```
lsvirtualenv
```

或者

```
workon
```



## 删除虚拟环境

```
rmvirtualenv env_django_1
```