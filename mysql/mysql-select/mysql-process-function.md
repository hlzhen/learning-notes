# Mysql流程控制函数
## 1.流程控制函数说明
```markdown
1. 流程处理函数可以根据不同的条件，执行不同的处理流程，可以在SQL语句中实现不同的条件选择。
2. Mysql中的流程处理函数主要包括IFO、IFNULL0和CASE(函数。
```
## 2.流程控制函数
| 函数                                                                   | 说明                                              |
| :--------------------------------------------------------------------- | :------------------------------------------------ |
| IF(value,value1,value2)                                                | 如果value的值为`true`,返回value1,否则返回value2。 |
| IFNULL(value1,value2)                                                  | 如果value1不为NULL，返回value1，否则返 回value2。 |
| CASE WHEN 条件1 THEN 结果1 WHEN 条件2 THEN 结果2..[ELSE resultn] END   | 相当于Java的if..else if..else..                   |
| CASE expr WHEN 常量值1 THEN 值1 WHEN 常量值1 THEN 值1...[ELSE 值n] END | 相当于Java的switch..case..                        |
```sql
#流程控制语句
SELECT 
	last_name,
	salary,
	IF(salary >= 9000,'高薪','小康')	'des'
FROM employees;

SELECT 
	last_name,
	IF(commission_pct IS NOT NULL,commission_pct,0)	'commission_pct'
FROM employees;

#IFNULL
SELECT 
	last_name,
	IFNULL(commission_pct, 0)	'commission_pct'
FROM employees;

#CASE WHEN THEN WHEN THEN ELSE END
SELECT 
	last_name,
	salary,
	CASE WHEN salary >= 15000 THEN 'A'
			 WHEN salary >= 10000 THEN 'B'
			 WHEN salary >= 5000 THEN 'C'
			 ELSE 'D' END 'lev'
FROM employees;

/*
    查询部门号为 10，20，30 的员工信息， 
    若部门号为10，则打印其工资的 1.1 倍， 
    20 号部门，则打印其工资的 1.2 倍， 
    30 号部门打印其工资的 1.3 倍
    其它部门则为1.4倍
*/
SELECT 
	employee_id,
	last_name,
	salary,
	department_id,
	CASE department_id WHEN 10 THEN salary * 1.1
										 WHEN 20 THEN salary * 1.2
										 WHEN 30 THEN salary * 1.3
										 ELSE salary * 1.4
										 END 'sal'
FROM employees;


/*
    查询部门号为 10，20，30 的员工信息， 
    若部门号为10，则打印其工资的 1.1 倍， 
    20 号部门，则打印其工资的 1.2 倍， 
    30 号部门打印其工资的 1.3 倍
*/
SELECT 
	employee_id,
	last_name,
	salary,
	department_id,
	CASE department_id WHEN 10 THEN salary * 1.1
										 WHEN 20 THEN salary * 1.2
										 WHEN 30 THEN salary * 1.3
										 END 'sal'
FROM 
	employees 
WHERE department_id IN(10,20,30)
```