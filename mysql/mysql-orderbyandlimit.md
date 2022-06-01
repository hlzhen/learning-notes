# Mysql排序与分页

## 1.排序
### 1.1 关键字
```markdown
 `ORDER BY`
    `ASC` : 表示升序(默认)，如不显示指定则按升序排序
    `DESC`: 表示降序
```
### 1.2 语法规则以及案例说明
```sql
/*
    写法规则: ORDER BY 列名或别名 ASC或DESC 
        SELECT * FROM employees ORDER BY salary;
        SELECT * FROM employees WHERE salary > 5000 ORDER BY salary;
        SELECT * FROM employees WHERE salary > 5000 ORDER BY salary DESC;
        从上查询语句中可看出，当有WHERE条件时ORDER BY 一定是写在WHERE条件后面，这样才符合SQL执行顺序。
*/
#一级排序: 查询工资大于5000的员工信息并按照工资进行降序排序
SELECT * FROM employees WHERE salary > 5000 ORDER BY salary DESC;

#多级排序: 查询工资大于5000的员工信息并按照工资进行降序排序，工资一致则按部门ID进行降序
SELECT * FROM employees WHERE salary > 5000 ORDER BY salary DESC, department_id DESC;
```

## 2.分页查询
```markdown
 5.7及以前版本 `LIMIT 偏移量,条目数(页大小)`
 1. 偏移量: 如不指定将从表中的第一条记录开始，第一条位置的偏移量为0，第二条则为1以此类推
 2. 条目数(页大小): 表示以偏移量为开始的记录到当前条目数的所有数据，如数据不满足条目数则有多少查多少
 3. 分页计算公式: LIMIT (currentPage - 1) * pageSize , pageSize

 8.0以后版本新特性 `LIMIT 条目数 OFFSET 偏移量`
 1. 高版本版本可兼容低版本，低版本并不能兼容高版本
 2. 从上可看出，OFFSET 前后代表的的数据与5.7的分页信息正好相反
```
```sql
#只查询一条记录
SELEct * FROM employees LIMIT 0,1;

#根据员工信息表进行分页查询，每页显示20条
SELEct * FROM employees LIMIT 0,20;

#查询第n页的员工信息，每页x条
SELEct * FROM employees LIMIT (n - 1) * x, x;

#查询工资大于6000的前10名员工信息
SELEct * FROM employees WHERE salary > 6000 ORDER BY salary DESC LIMIT 0,10;
```