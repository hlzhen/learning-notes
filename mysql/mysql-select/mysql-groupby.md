# Mysql GroupBy的使用

## 1.Group By的相关说明
```markdown
1. GROUP BY子句可以包含任意数目的列。这使得能对分组进行嵌套，为数据分组提供更细致的控制。
2. 如果在GROUP BY子句中嵌套了分组，数据将在最后规定的分组上进行汇总。
    - 换句话说，在建立分组时，指定的所有列都一起计算（所以不能从个别的列取回数据）。
3. GROUP BY 子句中列出的每个列都必须是检索列或有效的表达式（但不能是聚集函数）。
    - 如果在 SELECT 中使用表达式，则必须在GROUP BY子句中指定相同的表达式。不能使用别名。
4. 除聚集计算语句外，SELECT语句中的每个列都必须在GROUP BY子句中给出。
5. 如果分组列中具有NULL值，则NULL将作为一个分组返回。如果列中有多行NULL值，它们将分为一组。
6. GROUP BY子句必须出现在WHERE子句之后，ORDER BY子句之前。
```

## 2.案例
```sql
# 查询员工表中各个部门的平均工资
#查询员工表中各个部门的平均工资
SELECT 
	department_id,
	AVG(salary) 
FROM employees
GROUP BY department_id;

#查询各个部门与工种的平均工资
SELECT 
	department_id,
	job_id,
	AVG(salary) 
FROM employees
GROUP BY department_id,job_id;

SELECT 
	job_id,
	department_id,
	AVG(salary) 
FROM employees
GROUP BY job_id,department_id;


SELECT 
	department_id,
	AVG(salary) 'average' 
FROM employees 
GROUP BY department_id 
ORDER BY average
```

## 3.HAVING的使用
```sql
#查询部门id为10，20，30，40这4个部门中最高工资比10000高的部门信息
#方式1(推荐)
SELECT 
	department_id, 
	MAX(salary) max_salary
FROM employees
WHERE
	department_id IN(10,20,30,40)
GROUP BY department_id
HAVING max_salary > 10000 

#方式2
SELECT 
	department_id, 
	MAX(salary) max_salary
FROM employees
GROUP BY department_id
HAVING max_salary > 10000 AND department_id IN(10,20,30,40)
```