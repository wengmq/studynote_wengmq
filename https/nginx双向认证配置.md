# [NGINX 配置本地HTTPS(双向认证)](https://www.cnblogs.com/xiao987334176/p/11041241.html)

# 一、SSL协议加密方式

- SSL协议即用到了对称加密也用到了非对称加密(公钥加密)，在建立传输链路时，SSL首先对对称加密的密钥使用公钥进行非对称加密，链路建立好之后，SSL对传输内容使用对称加密。 
  1.对称加密 
  速度高，可加密内容较大，用来加密会话过程中的消息。 
  2.公钥加密 
  加密速度较慢，但能提供更好的身份认证技术，用来加密对称加密的密钥。

## **单向认证** 

Https在建立Socket连接之前，需要进行握手，具体过程如下： 

 ![img](https://img2018.cnblogs.com/blog/1341090/201906/1341090-20190617180105814-1473343757.png)

 

1、客户端向服务端发送SSL协议版本号、加密算法种类、随机数等信息。 
2、服务端给客户端返回SSL协议版本号、加密算法种类、随机数等信息，同时也返回服务器端的证书，即公钥证书 
3、客户端使用服务端返回的信息验证服务器的合法性，包括：

- 证书是否过期
- 发型服务器证书的CA是否可靠
- 返回的公钥是否能正确解开返回证书中的数字签名
- 服务器证书上的域名是否和服务器的实际域名相匹配

验证通过后，将继续进行通信，否则，终止通信 
4、客户端向服务端发送自己所能支持的对称加密方案，供服务器端进行选择 
5、服务器端在客户端提供的加密方案中选择加密程度最高的加密方式。 
6、服务器将选择好的加密方案通过明文方式返回给客户端 
7、客户端接收到服务端返回的加密方式后，使用该加密方式生成产生随机码，用作通信过程中对称加密的密钥，使用服务端返回的公钥进行加密，将加密后的随机码发送至服务器 
8、服务器收到客户端返回的加密信息后，使用自己的私钥进行解密，获取对称加密密钥。 在接下来的会话中，服务器和客户端将会使用该密码进行对称加密，保证通信过程中信息的安全。

 

## **双向认证** 

双向认证和单向认证原理基本差不多，只是除了客户端需要认证服务端以外，增加了服务端对客户端的认证，具体过程如下： 

 ![img](https://img2018.cnblogs.com/blog/1341090/201906/1341090-20190617180109757-1179347494.png)

 

**1、客户端向服务端发送SSL协议版本号、加密算法种类、随机数等信息。** 
**2、服务端给客户端返回SSL协议版本号、加密算法种类、随机数等信息，同时也返回服务器端的证书，即公钥证书** 
**3、客户端使用服务端返回的信息验证服务器的合法性，包括：** 

- - 证书是否过期
  - 发型服务器证书的CA是否可靠
  - 返回的公钥是否能正确解开返回证书中的数字签名
  - 服务器证书上的域名是否和服务器的实际域名相匹配

验证通过后，将继续进行通信，否则，终止通信 
4、服务端要求客户端发送客户端的证书，客户端会将自己的证书发送至服务端 
5、验证客户端的证书，通过验证后，会获得客户端的公钥 
6、客户端向服务端发送自己所能支持的对称加密方案，供服务器端进行选择 
7、服务器端在客户端提供的加密方案中选择加密程度最高的加密方式 
8、将加密方案通过使用之前获取到的公钥进行加密，返回给客户端 
9、客户端收到服务端返回的加密方案密文后，使用自己的私钥进行解密，获取具体加密方式，而后，产生该加密方式的随机码，用作加密过程中的密钥，使用之前从服务端证书中获取到的公钥进行加密后，发送给服务端 
10、服务端收到客户端发送的消息后，使用自己的私钥进行解密，获取对称加密的密钥，在接下来的会话中，服务器和客户端将会使用该密码进行对称加密，保证通信过程中信息的安全。

 

# 二、**Linux系统下生成证书**

**环境说明:**

| ip            | 操作系统       | 角色   |
| ------------- | -------------- | ------ |
| 192.168.0.162 | Ubuntu 16.04.5 | server |
| 192.168.0.120 | Ubuntu 16.04.5 | client |

 

 

 

 

 

**请确保server已经安装了Nginx**

## **生成 CA 私钥**

\1. 生成一个 CA 私钥: ca.key

```
mkdir /etc/nginx/keys/
cd /etc/nginx/keys/
openssl genrsa -out ca.key 4096
```

 

Generating RSA private key, 4096 bit long modulus
................................................................++
....................++
e is 65537 (0x10001)

 

## **生成CA 的数字证书**

\2. 生成一个 CA 的数字证书: ca.crt

```
openssl req -new -x509 -days 3650 -key ca.key -out ca.crt
```

 

Generating RSA private key, 4096 bit long modulus
................................................................++
....................++
e is 65537 (0x10001)
root@[ubuntu:/etc/nginx/keys#](http://ubuntu/etc/nginx/keys) openssl req -new -x509 -days 3650 -key ca.key -out ca.crt
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
\-----
Country Name (2 letter code) [AU]:CN
State or Province Name (full name) [Some-State]:Shanghai
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Team
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:Certificate Authority 2019
Email Address []:

 

======== 服务端保存 server.key，提供 server.csr，签名生成 server.crt ========

## **生成 server 端的私钥**

\1. 生成 server 端的私钥: server.key

```
openssl genrsa -out server.key 4096
```

 

Generating RSA private key, 4096 bit long modulus
.........++
....++
e is 65537 (0x10001)

 

### 生成 server 端数字证书请求

\2. 生成 server 端数字证书请求: server.csr

```
openssl req -new -key server.key -out server.csr
```

 

You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
\-----
Country Name (2 letter code) [AU]:CN
State or Province Name (full name) [Some-State]:Shanghai
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Sidien Test
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:192.168.0.162
Email Address []:

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:

 

**注意：由于使用ip地址访问的，所以Common Name，输入ip即可。**

**如果使用域名访问，那么这一步，必须是域名才行！**

 

### 用 CA 私钥签发 server 的数字证书

\3. 用 CA 私钥签发 server 的数字证书: server.crt

```
openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt -days 3650
```

 

Signature ok
subject=/C=CN/ST=Shanghai/O=Sidien Test/CN=192.168.0.162
Getting CA Private Key

 

======== 终端保存 client.key，提供 client.csr，签名生成 client.crt ========

## **生成 client 端的私钥**

### **生成客户端的私钥与证书**

\1. 生成客户端的私钥与证书: client.key

```
openssl genrsa -out client.key 4096
```

 

Generating RSA private key, 4096 bit long modulus
....++
...................................................................................................................................................................................................++
e is 65537 (0x10001)

 

### **生成 client 端数字证书请求**

\2. 生成 client 端数字证书请求: client.csr

```
openssl req -new -key client.key -out client.csr
```

 

You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
\-----
Country Name (2 letter code) [AU]:CN
State or Province Name (full name) [Some-State]:Shanghai
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Test
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:GP1700
Email Address []:

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:

 

### **用 CA 私钥签发 client 的数字证书**

\3. 用 CA 私钥签发 client 的数字证书: client.crt

```
openssl x509 -req -in client.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out client.crt -days 3650
```

 

Signature ok
subject=/C=CN/ST=Shanghai/O=Byzoro Test/CN=GP1700_v3.1.0
Getting CA Private Key

 

\4. 查看文件

```
root@ubuntu:/etc/nginx/keys# ls
ca.crt  ca.key  ca.srl  client.crt  client.csr  client.key  server.crt  server.csr  server.key
```

 

# 三、如何配置nginx

创建nginx配置文件

```
cd /etc/nginx/sites-enabled
vim https.conf
```

 

内容如下：

```
server {
        listen 443;
        server_name localhost;
        ssl on;
        ssl_certificate /etc/nginx/keys/server.crt;#配置证书位置
        ssl_certificate_key /etc/nginx/keys/server.key;#配置秘钥位置
        ssl_client_certificate /etc/nginx/keys/ca.crt;#双向认证
        ssl_verify_client on; #双向认证
        ssl_session_timeout 5m;
        ssl_protocols SSLv2 SSLv3 TLSv1 TLSv1.1 TLSv1.2; #按照这个协议配置
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE; #按照这个套件配置
        ssl_prefer_server_ciphers on;
        root html;
        index index.html;
        location / {
                try_files $uri $uri/ =404;
        }
}
```



**注意：这里使用的是ip地址访问，如果使用域名访问，请修改 server_name 为域名地址**

 

重载配置

```
nginx -s reload
```

 

至此，nginx的https就可以使用了，默认443端口

 

# 四、验证

## **浏览器测试**

使用https访问页面

https://192.168.0.162/

![img](http://wiki.smartriver.com.cn/download/attachments/5865604/image2019-6-3%2017%3A27%3A16.png?version=1&modificationDate=1559554036000&api=v2)

 

展开，点击继续

![img](http://wiki.smartriver.com.cn/download/attachments/5865604/image2019-6-3%2017%3A27%3A41.png?version=1&modificationDate=1559554061000&api=v2)

 

效果如下：

![img](http://wiki.smartriver.com.cn/download/attachments/5865604/image2019-6-17%2017%3A24%3A12.png?version=1&modificationDate=1560763453083&api=v2)

提示需要证书才行，说明双向认证是正常的！

 

## linux测试

## 查看curl版本

```
curl -V
```

 

curl 7.47.0 (x86_64-pc-linux-gnu) libcurl/7.47.0 GnuTLS/3.4.10 zlib/1.2.8 libidn/1.32 librtmp/2.3
Protocols: dict file ftp ftps gopher http https imap imaps ldap ldaps pop3 pop3s rtmp rtsp smb smbs smtp smtps telnet tftp 
Features: AsynchDNS IDN IPv6 Largefile GSS-API Kerberos SPNEGO NTLM NTLM_WB SSL libz TLS-SRP UnixSockets

 

**请确保curl版本不能低于 7.47版本，否则会出现：**

**400 No required SSL certificate was sent**

 

## 复制证书

```
cd /tmp/
scp -r 192.168.0.162:/etc/nginx/keys .
```

 

curl测试

```
curl --cacert ca.crt --cert client.crt --key client.key --tlsv1.2 https://192.168.0.162
```

 

输出：

如果有输出 Welcome to nginx! ，说明访问正常！

如果要代理公司认证服务，比如：192.168.0.11:30014

修改 https.conf



```
upstream auth { 
      server 192.168.0.11:30014;
}
server {
        listen 443;
        server_name localhost;
        ssl on;
        ssl_certificate /etc/nginx/keys/server.crt;
        ssl_certificate_key /etc/nginx/keys/server.key;
        ssl_client_certificate /etc/nginx/keys/ca.crt;#双向认证
        ssl_verify_client on; #双向认证
        ssl_session_timeout 5m;
        ssl_protocols SSLv2 SSLv3 TLSv1 TLSv1.1 TLSv1.2; #按照这个协议配置
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE; #按照这个套件配置
        ssl_prefer_server_ciphers  on;
        root  html;
        index index.html index.htm;
        location / {
                #try_files $uri $uri/ =404;
                proxy_pass http://auth;
                proxy_set_header Host $host:$server_port;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_next_upstream http_502 http_504 error timeout invalid_header; 
        }
}
```



使用curl，发送post请求

```
curl --cacert ca.crt --cert client.crt --key client.key --tlsv1.2 -H 'Content-type':'application/json' -d '{"userid":"123","duration":"3307"' https://192.168.0.162/Auth
```

输出：

```
{"code":"200","data":"","error":""}
```

 

输出上面那段信息，说明访问正常。

 

本文参考链接：

https://www.cnblogs.com/isylar/p/10002117.html

https://blog.csdn.net/qq_25406669/article/details/80596664