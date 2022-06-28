# 数据库的创建与管理

## 1.创建数据库
```sql
#数据库的创建
create database mytest1;
SHOW CREATE DATABASE mytest1;

#创建数据库并设置字符集 如果没有显示的指定字符集则用数据库默认字符集
CREATE DATABASE mytest2 CHARACTER SET 'utf8mb4';
SHOW CREATE DATABASE mytest2;

#创建数据库(如果不存在则创建)并指定字符集.存在则不进行创建(不替换原始数据库，推荐使用该方式创建数据库)
CREATE DATABASE IF NOT EXISTS mytest2 CHARACTER SET 'utf8mb4';
```

## 2.管理数据库
```sql
#查看当前连接中的数据库
SHOW DATABASES;

#切换数据库
USE atguigudb;

#查看当前数据库有哪些表
SHOW TABLES;

#查看当前使用的数据库
SELECT DATABASE() FROM DUAL;

#查看指定数据以下的表
SHOW TABLES FROM mysql;

#查看创建库的语句信息
SHOW CREATE DATABASE mysql;
```

## 3.修改数据库
```sql
#修改数据库，数据库名不能修改(可视化工具之所以可以更改是把原始表数据与结构复制到一个新表，然后在删除旧表)
ALTER DATABASE mytest3 CHARACTER SET 'utf8mb4';
SHOW CREATE DATABASE mytest3;
```

## 3.删除数据库
```sql
#删除数据库
DROP DATABASE mytest1;
SHOW DATABASES;

#删除数据库，如果存在的话
DROP DATABASE IF EXISTS mytest1;
```