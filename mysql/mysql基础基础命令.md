# mysql基础命令



### 连接本地的mysql数据库

```
 mysql -uroot -p
```



### 显示所有数据库

```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| djangoblog         |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
```



### 进入某个数据库

```
mysql> use djangoblog
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
```



### 显示该数据库下的所有表

```
mysql> show tables;
+------------------------------------+
| Tables_in_djangoblog               |
+------------------------------------+
| accounts_bloguser                  |
| accounts_bloguser_groups           |
| accounts_bloguser_user_permissions |
| auth_group                         |
+------------------------------------+
4 rows in set (0.00 sec)
```



## 查看表结构

```
mysql> desc auth_group;
```

