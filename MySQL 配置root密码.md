安装好的mysql环境是没有设置root密码的，使用一般的方法设置密码都改变不了。
这里记录一下，今天在mysql8上设置成功。
# 查看密码为空并设置密码
---
```shell
# 无需密码直接连接上mysql
sudo mysql -u root
```

```sql
USE mysql;
SELECT user, authentication_string FROM user WHERE user='root';

// 输出的密码是空的
+------+-----------------------+ 
| user | authentication_string | 
+------+-----------------------+ 
| root |                       | 
+------+-----------------------+ 
1 row in set (0.00 sec)

// 设置新密码
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123321';
```
设置成功：
```sql
mysql> SELECT user, authentication_string FROM user WHERE user='root';
+------+-------------------------------------------+
| user | authentication_string                     |
+------+-------------------------------------------+
| root | *437F1809645E0A92DAB553503D2FE21DB91270FD |
+------+-------------------------------------------+
1 row in set (0.00 sec)
```

# 连接mysql，验证输入密码
---
```shell
sudo mysql -u root -p
Enter password: 123321

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 37
Server version: 8.0.36-0ubuntu0.20.04.1 (Ubuntu)

Copyright (c) 2000, 2024, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
>mysql
```