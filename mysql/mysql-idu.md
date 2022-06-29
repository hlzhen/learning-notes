# Mysql增删改操作

## 1.准备工作
```sql
#创建表如果不存在的话
CREATE TABLE IF NOT EXISTS emp_001(
    id INT,
    `name` VARCHAR(32),
    birth_date DATE,
    salary DOUBLE(10,2)
);

#查看表结构
DESC emp_001;
```


## 2.添加表数据
```sql
`注意`:添加数据一定按照字段约束进行取值

/*
    #方式1:
        严格按照表字段顺序(需要熟悉表字段顺序)
        语法 INSERT INTO 表名 VALUES(字段名,字段名,...)
*/
INSERT INTO emp_001 VALUES(1, 'Tom', '2001-07-07', 3700);

/*
    方式2:
        按照自定义表字段顺序(推荐此方式，但表字段顺序还是按照表定义来)
        语法 INSERT INTO 表名(表字段,表字段,...) VALUES(字段名,字段名,...)
*/
INSERT INTO emp_001(id,birth_date,`name`,salary) VALUES(2, '2001-07-07', 'Jeck', 3700);


/*
    多数据添加(推荐写法):
        语法：INSERT INTO 表名(字段名,字段名,...) VALUES(字段名,字段名,...),(字段名,字段名,...),(字段名,字段名,...)
        VALUES后多个括号表示多条记录
*/
INSERT INTO 
    emp_001(id, birth_date, `name`, salary) 
VALUES
    (3, '2001-06-07', 'Kom', 39000),
    (4, '2001-11-11', 'Lok', 9700),
    (5,字段名,...);
```