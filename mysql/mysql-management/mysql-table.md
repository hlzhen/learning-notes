# Mysql数据表的创建与管理

## 1.创建数据表
```sql
/*
    TODO: 表的主键,自增,默认值,数据类型,字符集,索引
*/
USE 库名;

#注意如果创建表没有指定字符集则用当前库默认的字符集,如库没有指定则看mysql默认配置的字符集
CREATE TABLE IF NOT EXISTS myemp1(
	id INT,
	emp_name VARCHAR(20),
	hire_date DATE
);

#查看表结构
DESC myemp1;
SHOW CREATE TABLE myemp1;
```

## 2.修改数据表
### 2.1 添加表字段
```sql
#新增表字段(默认添加到最后位置)
ALTER TABLE myemp1 ADD salary DOUBLE(10,2);

#添加一个字段到表中的第一个位置
ALTER TABLE myemp1 ADD phone_number VARCHAR(20) FIRST;

#指定位置添加
ALTER TABLE myemp1 ADD email VARCHAR(20) emp_name;
```

### 2.2 修改表字段
```sql
#修改表字段，默认值(缺省值)，长度
#数据类型一般在定义时就确定好，一般不建议修改数据类型，尤其是有一定数据的时候
ALTER TABLE myemp1 MODIFY email VARCHAR(25);


/*
	修改默认值
	注意：
	修改时原始信息需要同步修改，不能只单改一项
	如：ALTER TABLE myemp1 MODIFY email VARCHAR(35) ;
	如下默认值默认为空字符串，那么如上修改长度后默认值则为null
*/
ALTER TABLE myemp1 MODIFY email VARCHAR(25) DEFAULT '';


#重命名字段（注意与修改的问题）
ALTER TABLE myemp1 CHANGE salary monthly_salary DOUBLE(10,2);
```

### 2.3 删除表字段
```sql
ALTER TABLE myemp1 DROP COLUMN def_email;
```

## 3.重命名表
```sql
#方式1
RENAME TABLE myemp1 TO myemp001;


#方式2
ALTER TABLE myemp001 RENAME TO myemp1;
```

## 4.清空表
```sql
#清空表只删除表数据，表结构保留，数据不可回滚
TRUNCATE TABLE myemp1;
```

## 5.删除表
```sql
#删除表结构与数据，释放表空间
DROP TABLE myemp1;

DROP TABLE IF EXISTS myemp1;
```