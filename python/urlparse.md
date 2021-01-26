# urlparse

**一 点睛**

urllib库里还提供了parse这个模块，它定义了处理URL的标准接口，例如实现URL各部分的抽取、合并以及链接转换。

它支持如下协议的URL处理：file、ftp、gopher、hdl、http、https、imap、mailto、 mms、news、nntp、prospero、rsync、rtsp、rtspu、sftp、 sip、sips、snews、svn、svn+ssh、telnet和wais。

本篇详细介绍urlparse()。

**二 urlparse()详解**

安装

```
$ pip3 install urllib3
```



使用说明：

```
$ python3
Python 3.8.2 (default, Nov  4 2020, 21:23:28)
[Clang 12.0.0 (clang-1200.0.32.28)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from urllib.parse import urlparse
>>>
>>> result = urlparse('http://www.baidu.com/index.html;user?id=5#comment')
>>>
>>>
>>> print(type(result), result)
<class 'urllib.parse.ParseResult'> ParseResult(scheme='http', netloc='www.baidu.com', path='/index.html', params='user', query='id=5', fragment='comment')

#返回结果ParseResult实际上是一个元组，我们可以用索引顺序来获取，也可以用属性名获取。
>>> result.path
'/index.html'
>>> result.netloc
'www.baidu.com'
```

