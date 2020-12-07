# docker-compose命令

### 进入测试环境（进入容器）miniCDN下

- docker-compose exec edge|parent|src bash

### 关闭测试环境

- docker-compose down

### 开启测试环境

-  docker-compose up



### cd到 bstc/jupyter/docker 和 bstc/miniCDN/docker  执行：docker-compose up -d --build --force

- -d:后台执行 ; 
- --force:强制执行 ; 
- --build:创建