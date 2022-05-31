# Mysql查询

## 1.基本查询
```sql
/*
 1. 基本查询结构 select 字段名1,字段2,... from 表名
 2. 当然也可以使用伪表做一些逻辑运算，如下:
    select 1+1;
    select 1+1,3*3;
    select 1 + 1,3*2 FROM DUAL; #dual:伪表

    * 符号说明： 在上述场景中表示相乘
*/

# 查询employees表中全部数据(不推荐)
SELECT * FROM employees;

#从员工表中查询员工ID,名字,工资(推荐写法)
SELECT employee_id,first_name,salary FROM employees;
```

## 2.别名
```sql
/*
  列的别名:
    顾名思义给列取别名，通过别名代表这个列。
    写法1: 直接在某个列名后面用空格隔开然后接上别名(不推荐，容易误导为某个列名)。
    写法2: 直接在某个列名后面通过 as 别名 来表示这个列所取的别名(推荐写法)。
    如下：
*/
SELECT employee_id id,first_name,salary FROM employees;
SELECT employee_id as id,first_name,salary FROM employees;

/*
  表的别名:
    顾名思义给表取别名，通过别名代表这个表的名称。
    写法方式与列的别名一致。
    需要注意的是如给表取别名后在查询某些列的时候需要通过 表别名.列名 的方式查询
*/
SELECT e.employee_id as id, e.first_name,e.salary FROM employees e;
SELECT e.employee_id as id, e.first_name,e.salary FROM employees as e;
```

## 3.去重
```sql
/*
    关键字: DISTINCT
*/
SELECT DISTINCT department_id FROM employees;
```

## 4.空值参与运算
```sql
/*
    空值:null
    如直接参与运算的话则运算结果为null
        SELECT employee_id,salary,(salary * (1 + commission_pct) * 12) "年工资" FROM employees;
    空值运算解决方案IFNULL(expr1,expr2)
        参数1 表示字段名也就是列名
        参数2 表示用什么代替参数1
        结合一起就是当参数1为空值null的时候启用参数2代替
*/
SELECT employee_id,salary,(salary * (1 + IFNULL(commission_pct,0)) * 12) "年工资" FROM employees;
```

## 5.显示表结构
```sql
#显示表结构 DESC 是 DESCRIBE 的简写
DESCRIBE employees;
DESC employees;
```

## 6.过滤查询
```sql
/*
    语法: select 字段名 from 表名 where 条件
    例如: 查询所在部门为100的所有员工信息
*/
SELECT * FROM employees WHERE department_id = 100
```

## 7.基本运算符
```sql
/*
    加(+)、减(-)、乘(*)、除(/)、取余(%)
        SELECT 1+2*8/2%2 FROM DUAL; 
        结果:1.0000
    注意:
        在sql中的运算的优先级也是先乘除后加减
        如果除数为0那么整个运算结果则为null
*/
SELECT 1 + 2 * 8 / 2 % 2 FROM DUAL;
```

## 8.比较运算符
### 8.1 符号类比较运算符
| 运算符号 | 名称     |
| :------- | :------- |
| =        | 等于     |
| <=>      | 安全等于 |
| <>(!=)   | 不等于   |
| <        | 小于     |
| <=       | 小于等于 |
| >        | 大于     |
| >=       | 大于等于 |
```sql
/*
    符号类比较运算符
    注意:
        字符串存在隐式转换，如果转换数值不成功则看作为0
        如纯粹字符串比较的换就不存在隐式转换
        如与null进行比较那么结果则为null
*/
SELECT 1 = 1, 1 = '1', 1 = 'a', 0 = 'a' FROM DUAL;    #结果:1,1,0,1
SELECT 'a' = 'a','ab' = 'ab','a' =' b' FROM DUAL;     #结果:1,1,0
SELECT 'a' = 'null','ab' = null FROM DUAL;            #结果:0,null
SELECT * FROM employees WHERE commission_pct = NULL;  #结果:无任何结果


SELECT 'a' <=> 1, 'ab' <=> null,null <=> null FROM DUAL;  #结果:0,0,1
```
### 8.2 非符号类比较运算符
| 运算符号    | 名称             | 解释                                                                            |
| :---------- | :--------------- | :------------------------------------------------------------------------------ |
| IS NULL     | 为空运算         | 判断值、字符串、或者表达式  {% em color="#EEEED1" %} 是否为空 {% endem %}。     |
| IS NOT NULL | 不为空运算       | 判断值、字符串、或者表达式   {% em color="#EEEED1" %} 是否不为空 {% endem %}。 |
| LEAST       | 最小值运算       | 在多个值中返回 {% em color="#EEEED1" %}最小值{% endem %}。                      |
| GREATEST    | 最大值运算       | 在多个值中返回 {% em color="#EEEED1" %}最大值{% endem %}。                      |
| BETWEEN AND | 两值之间的运算符 | 判断一个值 {% em color="#EEEED1" %}是否在两值之间(包含值){% endem %}。                  |
| ISNULL      | 为空运算         | 判断值、字符串、或者表达式 {% em color="#EEEED1" %}是否为空{% endem %}。        |
| IN          | 属于运算符       | 判断一个值 {% em color="#EEEED1" %}是否属于{% endem %}列表中的某个值。          |
| NOT IN      | 不属于运算符     | 判断一个值是 {% em color="#EEEED1" %}否不属于{% endem %}列表中的某个值。        |
| LIKE        | 模糊匹配运算符   | 判断一个值{% em color="#EEEED1" %}是否符合模糊匹配{% endem %}的规则。           |
| REGEXP      | 正则表达式运算符 | 判断一个值{% em color="#EEEED1" %}是否符合正则表达式{% endem %}的规则。         |
| RLIKE       | 正则表达式运算符 | 判断一个值{% em color="#EEEED1" %}是否符合正则表达式{% endem %}的规则。         |
```sql
/*
    非符号类比较运算符
    注意:
        BETWEEN 条件1 AND 条件2 其条件1的值必须小于条件2这样才成立
        
*/
#从员工表中查询奖金率为空的员工
SELECT * FROM employees WHERE commission_pct IS NULL;

#从员工表中查询奖金率为空的员工
SELECT * FROM employees WHERE ISNULL(commission_pct);

#从员工表中查询奖金率不为空的员工
SELECT * FROM employees WHERE commission_pct IS NOT NULL;


#最小值(LEAST)与最大值(GREATEST)
SELECT LEAST('1','o') FROM DUAL;        #结果:1
SELECT GREATEST('1','2','3') FROM DUAL; #结果:3


#(BETWEEN AND)查询工资在6000-8000之间的员工
SELECT * FROM employees WHERE salary BETWEEN 6000 AND 8000;

#(BETWEEN AND)查询工资不在6000-8000之间的员工
SELECT * FROM employees WHERE salary NOT BETWEEN 6000 AND 8000;


#(IN/NOT IN)查询所在部门为10，20，30的员工信息
SELECT * FROM employees WHERE department_id IN(10,20,30);

#(IN/NOT IN)查询不在部门为10，20，30的员工信息
SELECT * FROM employees WHERE department_id NOT IN(10,20,30);


/*
    LIKE模糊查询：
        '%': 匹配任何数目(0个，1个或多个)的字符，甚至包括零字符。
        '_': 只能匹配一个字符。
    注意: %在前表示以什么结尾，%在后表示以什么开头
*/
#(LIKE)模糊查询 查询姓氏包含a的员工
SELECT last_name FROM employees WHERE last_name LIKE '%a%';

#(LIKE)模糊查询 查询姓氏以a开头的员工
SELECT last_name FROM employees WHERE last_name LIKE 'a%';

#(LIKE)模糊查询 查询姓氏以a结尾头的员工
SELECT last_name FROM employees WHERE last_name LIKE '%a';

#(LIKE)模糊查询 查询姓氏包含a且包含e的员工
SELECT last_name FROM employees WHERE last_name LIKE '%a%' AND last_name LIKE '%e%';
#此写法少见(不推荐)
SELECT last_name FROM employees WHERE last_name LIKE '%a%e%' OR last_name LIKE '%e%a%';

#(LIKE)模糊查询 查询姓氏第二字符是a的员工
SELECT last_name FROM employees WHERE last_name LIKE '_a%';

#(LIKE)模糊查询 查询姓氏第二字符是_且第三个字符是a的员工，\代表转义
SELECT last_name FROM employees WHERE last_name LIKE '_\_a%';


#(REGEXP/RLIKE)正则表达式
SELECT 'shjhkjt' REGEXP '^s', 'shjhkjt' REGEXP 't$', 'shjhkjt' REGEXP 'hk' FROM DUAL;

#(REGEXP/RLIKE)正则表达式 .表示不确定字符，匹配shjhkjt是否有k.t格式的字符
SELECT 'shjhkjt' REGEXP 'k.t' FROM DUAL;

```