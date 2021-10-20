# nginx的if陷阱

在编辑语言中if是一个分支执行执行，根据条件执行不同的代码块，而在nginx中完全不是这样，nginx中的if跟编程中的if没有任何相似性。

if到底是怎么执行的看了很多资料还是迷迷糊糊，按照官方的建议不要在if块中包含任何非rewrite模块的中指梳，否则会发生莫名其秒的问题。本来是应该禁止在if块中写非rewrite指令的，但因为兼容性问题暂没有这么做。

看了很多例子我总结了以下几点

1. 多个if只有最后一个if块中的非rewtire指令会被执行（从后面往前执行的？）
2. if执行后非rewrite指令后会跳过后面的rewire指令的执行，其它指令中有rewrite中相关的逻辑也不会执行。
3. if会导致try_files不能执行，当有if的时候try_files无论如何都不会执行，不管是空if还是只有rewrite的if块，但是有些语句又是可以和if共存的，实在是莫名其妙，这找不到任何原理可以解释。
4. if会导致proxy_pass执行出错
5. if包含非rewrite指令时，会直接跳到其它阶段去执行，rewrite阶段的其它代码就中止了，似乎nginx各个指令是回调执行的，进入content后就不会再回调rewrite中的代码了，而if又是立即执行的指令。执行哪些指令会中止rewrite最好自己去试。
6. 一些指令继承到if块中，哪些会继承哪些不会只能自己去试，如果有非root指令即使承承可能也不执行。
7. if不能嵌套

一些指令，像limit_rate，add_header是可以安全与if一起使用的，甚至if外的return执行也不受影响。我猜是这些指令执行后又跑回了rewrite，这与上面的理论又是矛盾的。或者只是部分指令会中止rewrite的执行，比如echo命令。

反正就是if命令没有任何规则可言，尽可能避开if命令的使用，如果必须用就要自己去测试各种陷阱。

参考：

[https://www.kawabangga.com/posts/3239](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.kawabangga.com%2Fposts%2F3239)

[https://github.com/openresty/echo-nginx-module](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fopenresty%2Fecho-nginx-module)

[http://nginx.org/en/docs/http/ngx_http_rewrite_module.html#if](https://links.jianshu.com/go?to=http%3A%2F%2Fnginx.org%2Fen%2Fdocs%2Fhttp%2Fngx_http_rewrite_module.html%23if)

https://www.nginx.com/resources/wiki/start/topics/depth/ifisevil/