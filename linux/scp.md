# SCP

[**linux采用scp命令拷贝文件到本地，拷贝本地文件到远程服务器**](https://www.cnblogs.com/clovershell/p/9870603.html)

参考：https://www.cnblogs.com/clovershell/p/9870603.html



**// 假设远程服务器IP地址为 192.168.1.100**

 

**1.从服务器复制文件到本地：**

scp root@192.168.1.100:/data/test.txt /home/myfile/

补充：多文件拷贝

scp root@192.168.1.100:/data/\{test1.txt,test2.cpp,test3.bin,test.*\} /home/myfile/

root@192.168.1.100  root是目标服务器（有你需要拷贝文件的服务器）的用户名，192.168.1.100是IP地址，后面紧跟的 “：” 不要忘记，/data/test.txt（多文件还有test1.txt，test2.cpp，test3.bin，test.a，test.c等） 是目标服务器中你要拷贝文件的地址，接一个空格，后面的 /home/myfile/ 是本地接收文件的地址。

 

**2.从服务器复制文件夹到本地：**

scp -r root@192.168.1.100:/data/ /home/myfile/



Eg:

scp root@47.102.98.58:/usr/local/openresty/nginx/conf/bs_lua/all.host /root/wengmq/all.host

只需在前面加 -r 即可，就可以拷贝整个文件夹。

 

**3.从本地复制文件到服务器：**

scp /home/myfile/test.txt root@192.168.1.100:/data/

补充：多文件拷贝

scp /home/myfile/test1.txt test2.cpp test3.bin test.* root@192.168.1.100:/data/

scp /home/ops/xu root@47.102.98.58:/usr/local/openresty/nginx/xu

 

**4.从本地复制文件夹到服务器：**

scp -r /home/myfile/ root@192.168.1.100:/data/