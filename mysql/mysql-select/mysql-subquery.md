# Mysql 子查询

## 1.什么是子查询
```markdown
1. 子查询指一个查询语句嵌套在另一个查询语句内部的查询，这个特性从MySQL4.1开始引入。
2. 换句话说子查询就好比Java里面的循环套循环，子查询也是类似。
3. SQL 中子查询的使用大大增强了 SELECT 查询的能力，因为很多时候查询需要从结果集中获取数据，
    - 或者需要从同一个表中先计算得出一个数据结果，然后与这个数据结果（可能是某个标量，也可能是某个集合）进行比较。

# 子查询注意事项
1. 子查询（内查询）在主查询之前一次执行完成。 
2. 子查询的结果被主查询（外查询）使用 注意事项 
3. 子查询要包含在括号内 
4. 将子查询放在比较条件的石侧 
5. 单行操作符对应单行子查询，多行操作符对应多行子查询
```

## 2.简单子查询案例
```sql
#谁的工资比Abel高
#方式1: 两次查询得出结果
SELECT salary FROM employees WHERE last_name = 'Abel';
SELECT last_name,salary FROM employees WHERE salary > 11000;

#方式2: 自连接得出结果(并不代表所有类似需求都能演变成自连接)
SELECT 
	e2.last_name, 
	e2.salary
FROM employees e1,employees e2
WHERE 
	e2.salary > e1.salary
AND
	e1.last_name = 'Abel';
	
#方式3: 子查询
SELECT 
	last_name,
	salary 
FROM employees 
WHERE salary > (
		SELECT salary FROM employees WHERE last_name = 'Abel'
);
```

## 3.子查询例题
### 3.1 基本子查询
```sql
#1. 查询工资大于149号员工工资的员工信息
SELECT 
	employee_id,
	last_name,
	salary 
FROM employees
WHERE salary > (
	SELECT salary FROM employees WHERE employee_id = 149
);

#2.查询哪些员工工资信息等于各部门最低工资
SELECT 
	employee_id,
	last_name 
FROM 
	employees 
WHERE 
	salary IN(
		SELECT MIN(salary) FROM employees GROUP BY department_id
);

#3.返回其它job_id中比job_id为`IT_PROG`部门任一工资低的员工的员工号、姓名、job_id以及salary
SELECT 
	employee_id,
	last_name,
	job_id,
	salary
FROM employees
WHERE 
	job_id <> 'IT_PROG'
AND 
	salary < ANY(
	SELECT salary FROM employees WHERE job_id = 'IT_PROG'
);

#4.返回其它job_id中比job_id为`IT_PROG`部门所有工资低的员工的员工号、姓名、job_id以及salary
SELECT 
	employee_id,
	last_name,
	job_id,
	salary
FROM employees
WHERE 
	job_id <> 'IT_PROG'
AND 
	salary < ALL(
	SELECT salary FROM employees WHERE job_id = 'IT_PROG'
);

#5.查询平均工资最低的部门ID
#解法1
SELECT 
	department_id 
FROM employees 
GROUP BY department_id 
HAVING AVG(salary) = (
	SELECT 
		AVG(salary) avg_salary
	FROM employees 
	GROUP BY department_id 
	ORDER BY avg_salary
	LIMIT 0,1
)

#解法2
SELECT 
	department_id 
FROM employees 
GROUP BY department_id 
HAVING AVG(salary) = (
	SELECT
		MIN(avg_salary)
	FROM (
		SELECT 
			department_id,
			AVG(salary) avg_salary
		FROM employees 
		GROUP BY department_id 
	) t_dept_avg_salary
)

#解法3
SELECT 
	department_id 
FROM employees 
GROUP BY department_id 
HAVING AVG(salary) <= ALL (
		SELECT 
			AVG(salary) avg_salary
		FROM employees 
		GROUP BY department_id 
)


#6.查询员工巾工资大于本公司平均工资的员工的last name, salary和其department_id
SELECT 
	last_name, 
	salary, 
	department_id 
FROM employees 
WHERE 
	salary > ( 
		SELECT AVG( salary ) FROM employees 
	);

#7.查询员工巾工资大于本部门平均工资的员工的last name, salary和其department_id
#解1
SELECT 
	e1.last_name, 
	e1.salary, 
	e1.department_id 
FROM employees e1
WHERE 
	salary > ( 
		SELECT AVG( e2.salary ) FROM employees e2 WHERE department_id = e1.department_id 
	);

#解2
SELECT 
	e.last_name, 
	e.salary, 
	e.department_id 
FROM employees e,(
		SELECT 
			department_id,
			AVG(salary) avg_sal 
		FROM employees 
		GROUP BY department_id
) t_dept_avg_sal
WHERE 
	e.department_id = t_dept_avg_sal.department_id
AND
	e.salary > t_dept_avg_sal.avg_sal;
	
	
#8.查询员工的id,salary,按照department_name排序
SELECT 
	e.employee_id,
	e.salary
FROM employees e
ORDER BY (
	SELECT
		d.department_name
	FROM departments d
	WHERE 
		d.department_id = e.department_id
	)
 
 #连接查询解法
 SELECT 
	e.employee_id,
	e.salary,
	d.department_name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id
ORDER BY d.department_name;

/*
9.若employees表中employee_id与job_history表中employee_id相同的数目不小于2,
 输出这些相同id的员工的employee_id, last_name和其job_id
*/

SELECT 
	e.employee_id,
	e.last_name,
	e.job_id 
FROM employees e 
WHERE  (
	SELECT count(*) FROM job_history h WHERE h.employee_id = e.employee_id
) >= 2


#10.查询公司管理者的employee_id, last_name, job_id, department_id信息
#解1:连接查询
SELECT 
	DISTINCT mgr.employee_id,
	mgr.last_name,
	mgr.job_id,
	mgr.department_id
FROM employees emp,employees mgr
WHERE mgr.employee_id = emp.manager_id;

#解2:子查询
SELECT 
	employee_id,
	last_name,
	job_id,
	department_id
FROM employees 
WHERE 
	employee_id IN (
		SELECT DISTINCT manager_id FROM employees 
);

#解3: EXISTS
SELECT 
	e1.employee_id,
	e1.last_name,
	e1.job_id,
	e1.department_id
FROM employees e1
WHERE EXISTS (
		SELECT e2.manager_id FROM employees e2 WHERE e1.employee_id = e2.manager_id
);

#11.查询departments表中,不存在于employees表中的部门的department_id和department_name
#解1:连接查询
SELECT 
	d.department_id,
	d.department_name 
FROM departments d
LEFT JOIN employees e
ON d.department_id = e.department_id
WHERE e.department_id IS NULL;

#解2:NOT EXISTS
SELECT 
	d.department_id,
	d.department_name 
FROM departments d
WHERE NOT EXISTS (
 SELECT e.department_id FROM employees e WHERE d.department_id = e.department_id
);
```