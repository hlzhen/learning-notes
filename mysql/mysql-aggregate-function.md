# Mysql常用聚合函数

## 1.聚合函数概念
```markdown
1. 聚合函数(聚集、分组)函数，是对一组数据进行汇总的函数。
2. 函数输入的是一组数据的集合，输出的是单个值
```

## 2.常用函数
| 函数名称 | 说明                   |
| :------- | :--------------------- |
| AVG()    | 返回一组数据的平均值。 |
| SUM()    | 返回一组数据的总和。   |
| MIN()    | 返回一组数据的最小值。 |
| MAX()    | 返回一组数据的最大值   |
| COUNT()  | 返回总条数。           |
```sql
SELECT 
	AVG(salary),
	SUM(salary)
FROM employees;

SELECT 
	AVG(commission_pct),
	SUM(commission_pct)/COUNT(commission_pct),
	SUM(commission_pct)/107
FROM employees;

SELECT 
	MAX(salary),
	MIN(salary),
	MAX(hire_date),
	MIN(hire_date),
	MAX(last_name),
	MIN(last_name)
FROM employees;

#如传入某个具体的字段，则统计的数量不包含null值
SELECT COUNT(*) FROM employees;
```