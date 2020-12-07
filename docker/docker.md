# docker命令

### docker登录

```
docker login -u 账号 -p 密码 hub.bscstorage.com（登录的网址）
```



### 查看运行的docker镜像：docker ps

```
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                                              NAMES
7d6fe5ebe3b2        cdn                 "/share/entrypoint.sh"   4 weeks ago         Up 16 seconds                                                          docker_edge_1
d61f435b2021        cdn                 "/share/entrypoint.sh"   4 weeks ago         Up 16 seconds                                                          docker_parent_1
21534d245b48        cdn                 "/share/entrypoint.sh"   4 weeks ago         Up 16 seconds                                                          docker_src_1
6b05aac54a77        redis               "docker-entrypoint.s…"   4 weeks ago         Up 5 days           6379/tcp                                           docker_minicdn-redis_1
09a9886c83db        bstc_jupyter        "/share/entrypoint.sh"   4 weeks ago         Up 7 seconds        0.0.0.0:18000->8000/tcp, 0.0.0.0:19000->9000/tcp   docker_jupyter_1
```



