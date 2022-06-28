# Mysql多表查询

## 1.查询案例
![表结构](/assets/mysql/select_emp_dept_locaton.jpg)
>[!Tip]
> <p>以下多表查询案例来自于上图表结构。</p>


```sql
# 案例1: 显示所有员工部门信息以及员工所在城市
#方式1
SELECT 
	employees.first_name,
	employees.salary,
	departments.department_name,
	locations.city
FROM 
	employees,departments,locations 
WHERE 
	employees.department_id = departments.department_id
AND
	departments.location_id = locations.location_id;

#方式2
SELECT 
	emp.first_name,
	emp.salary,
	dept.department_name,
	lct.city
FROM 
	employees emp,departments dept,locations lct
WHERE 
	emp.department_id = dept.department_id
AND
	dept.location_id = lct.location_id;


#案例2: 显示'Lisa'的部门信息以及所在城市
SELECT 
	emp.first_name,
	emp.salary,
	dept.department_name,
	lct.city
FROM 
	employees emp,departments dept,locations lct
WHERE 
	emp.department_id = dept.department_id
AND
	dept.location_id = lct.location_id
AND 
    emp.first_name = 'Lisa';


#案例3: 显示前10条工资大于6000的员工信息(包含所在城市以及部门信息)并按工资降序
SELECT 
	emp.first_name,
	emp.salary,
	dept.department_name,
	lct.city
FROM 
	employees emp,departments dept,locations lct
WHERE 
	emp.department_id = dept.department_id
AND
	dept.location_id = lct.location_id
AND 
    emp.salary > 6000
ORDER BY emp.salary DESC
LIMIT 0,10;
```

>[!Tip]
> <p>{% em color="#EEEED1" %}通过多表查询可得出结论: 如果有n表实现多表查询，则需要至少n-1个连接条件。{% endem %}</p>
> <p>{% em color="#EEEED1" %}注意: 如多表查询不关联对应连接条件那么则会出现笛卡尔积(交叉连接)的数据错误。 {% endem %}</p>



## 2.多表查询分类
| 分类名称   | 解释                                                                                          |
| :--------- | :-------------------------------------------------------------------------------------------- |
| 等值连接   | 多表连接，关联关系条件 {% em color="#EEEED1" %}相等{% endem %} 的情况下进行的筛选查询。       |
| 非等值连接 | 多表连接，关联关系条件 {% em color="#EEEED1" %}不相等{% endem %} 的情况下进行的筛选查询。     |
| 自连接     | 自己与自己连接查询(自我引用)。                                                                |
| 非自连接   | 与自连接相关。                                                                                |
| 内连接     | 取出两张表中匹配到的数据，匹配不到的{% em color="#EEEED1" %}不保留{% endem %}。               |
| 外连接     | 取出连接表中匹配到的数据，匹配不到的{% em color="#EEEED1" %}也会保留{% endem %}，其值为NULL。 |

### 2.1 等值连接
```sql
#案例
SELECT 
	emp.first_name,
	emp.salary,
	dept.department_name,
	lct.city
FROM 
	employees emp,departments dept,locations lct
WHERE 
	emp.department_id = dept.department_id
AND
	dept.location_id = lct.location_id;
```

### 2.2 非等值连接、自连接、非自连接
![非等值查询结构](/assets/mysql/select_notequivalent.jpg)
```sql
#1.非等值连接: 查询员工的岗位级别
SELECT 
	emp.employee_id,
	emp.first_name,
	emp.salary,
	job.grade_level
FROM
	employees emp,job_grades job
WHERE
	emp.salary BETWEEN job.lowest_sal AND job.highest_sal
ORDER BY emp.salary DESC;

#2.自连接: 查询员工的上级管理者名称
SELECT 
	emp.employee_id,
	emp.first_name,
	emp.salary,
	mgr.first_name manager_name
FROM
	employees emp, employees mgr
WHERE
 	emp.manager_id = mgr.employee_id

#3.非自连接，只要不是与自己连接的多表查询都是非自连接
```

### 2.3 内连接
![Mysql内连接](/assets/mysql/mysql_inner_join.jpg)
```sql
#组合两个表中的记录，返回关联字段相符的记录，也就是两个表的交集部分
SELECT
	emp.employee_id,
	emp.last_name,
	dept.department_name 
FROM
	employees emp
INNER JOIN 
	departments dept 
ON 
	emp.department_id = dept.department_id;


#简写版
SELECT
	emp.employee_id,
	emp.last_name,
	dept.department_name 
FROM
	employees emp
JOIN 
	departments dept 
ON 
	emp.department_id = dept.department_id;
```

### 2.4 左外连接
![Mysql内连接](/assets/mysql/mysql_left_join.jpg)
```sql
SELECT
	emp.employee_id,
	emp.last_name,
	dept.department_name 
FROM
	employees emp
LEFT JOIN 
	departments dept 
ON 
	emp.department_id = dept.department_id;
```


### 2.5 右外连接
![Mysql内连接](/assets/mysql/mysql_right_join.jpg)
```sql
SELECT
	emp.employee_id,
	emp.last_name,
	dept.department_name 
FROM
	employees emp
RIGHT JOIN 
	departments dept 
ON 
	emp.department_id = dept.department_id;
```


### 2.6 其它
```markdown
# 关于Mysql中的其它的连接查询
1. `UNION`: 
	- 必须由两条或两条以上的SELECT语句组成，语句之间用关 键字UNION分隔。
	- UNION中的每个查询必须包含相同的列、表达式或聚集函数(不过各个列不需要以相同的次序列出)
	- 列数据类型必须兼容：类型不必完全相同，但必须是DBMS可以 隐含地转换的类型(例如，不同的数值类型或不同的日期类型)。
	- UNION从查询结果集中自动去除了重复的行。
2. `UNION ALL`:
	- UNION ALL为UNION的一种形式，它完成 WHERE子句完成不了的工作。
	- 如果确实需要每个条件的匹配行全 部出现（包括重复行），则必须使用UNION ALL而不是WHERE。


# INNER JOIN 、LEFT JOIN 、RIGHT JOIN三者之间的区别
1. ` INNER JOIN`:
	- 组合两个表中的记录，返回关联字段相符的记录，也就是两个表的交集部分
2. `LEFT JOIN`:
	- 组合两个表中的记录，返回以左表记录为基准的相关数据，如为null也会返回
2. `RIGHT JOIN`:
	- 组合两个表中的记录，返回以右表记录为基准的相关数据，如为null也会返回
```




