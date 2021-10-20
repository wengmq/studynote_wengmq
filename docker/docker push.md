# docker push 上传镜像到 hub.docker.com

1. 首先在hub.docker.com上面注册账号，获得账号和密码
2. 在终端上面docker login：（默认会登录到hub.docker.com）
   - docker login -u 账号名 -p 密码
3. 使用docker tag 打包镜像
   - 例如：docker tag 镜像名 账号名/镜像名:tag
     - docker tag hello-world wengmq/hello-world:1.0

4. Docker push 到远程仓库：
   - docker push wengmq/hello-world:1.0

