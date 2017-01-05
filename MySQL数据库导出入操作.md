#MySQL Windows导出导入数据库

使用mysqldump命令导出数据：

```
D:\Program Files\MySQL\MySQL Server 5.6\bin>mysqldump -hlocalhost -uroot -proot sinaweibo >D:\dev\mysql_bkp\sinaweibo.sql
```

将数据导入回数据库：

导入前需要创建对应名字的数据库

```
mysql> create database sinaweibo2;
```

```
D:\Program Files\MySQL\MySQL Server 5.6\bin>mysql -hlocalhost -uroot -proot sinaweibo2 < D:\dev\mysql_bkp\sinaweibo.sql
```

原因：导入数据时的默认编码（utf8）与导出文件的默认编码（utf8mb4）不一致。

解决办法：
```
D:\Program Files\MySQL\MySQL Server 5.6\bin>mysql -hlocalhost -uroot -proot --default-character-set=utf8mb4 sinaweibo2 < D:\dev\mysql_bkp\sinaweibo.sql
```