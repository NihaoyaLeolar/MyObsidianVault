---
tags:
  - mysql
---
在mysql中创建一个新用户，给navicat远程访问
navicat连接mysql时，如果用户不存在，或者用户密码错误，会1130错误
![[CleanShot 2024-02-12 at 22.35.55@2x.png|400]]
先进入mysql，如果没有设置root密码，以下直接回车进入。或者通过[[MySQL 配置root密码]]设置一下也行。
```shell
sudo mysql -u root -p
```
然后创建新用户nihaoya，且允许远程访问
```sql
mysql> CREATE USER 'nihaoya'@'%' IDENTIFIED BY '12345678';
Query OK, 0 rows affected (0.02 sec)

mysql> GRANT ALL PRIVILEGES ON *.* TO 'nihaoya'@'%';
Query OK, 0 rows affected (0.01 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)
```
检查设置成功
```sql
mysql> use mysql
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed


mysql> SELECT user, authentication_string FROM user WHERE user='nihaoya';
+---------+------------------------------------------------------------------------+
| user    | authentication_string                                                  |
+---------+------------------------------------------------------------------------+
| nihaoya | $A$005$~Tqp(nvo/a!./ehirC6BBGkGSCh1rQko616nCPacbfgu1hM9nMBrzwRE.2 |
+---------+------------------------------------------------------------------------+
1 row in set (0.00 sec)

```