# Mysql其它函数

## 1.加解密函数
### 1.1 加解密函数概述
```markdown
1. 加密与解密函数主要用于对数据库中的数据进行加密和解密处理，以防止数据被他人窃取。
2. 这些函数在保证敉据库安全时非常有用。
3. 一般来说比较敏感的数据会直接在客户端或者程序中进行加密在添加到数据库中
```

### 1.2 函数分类
| 函数                        | 说明                                                                                               |
| :-------------------------- | :------------------------------------------------------------------------------------------------- |
| PASSWORD(str)               | 返回字符串str的加密版本，41位长的字符串。加密结果`不可逆`，常用于用户的密码加密。8.0以后已弃用     |
| MD5(str)不可逆              | 返回字符串str的md5加密后的值，也是一种加密方式。若参数为NULL，则会返回NULL。                       |
| SHA(str)不可逆              | 从原明文密码str计算并返回加密后的密码字符串，当参数为NULL时，返回 NULL。SHA加密算法比MD5更加安全。 |
| ENCODE(value,password_seed) | 返回使用password_seed作为加密密码加密value，8.0以后已弃用 。                                       |
| DECODE(value,password_seed) | 返回使用password_seed作为加密密码解密value，8.0以后已弃用。                                        |

```sql
#加解密
SELECT 
	MD5('mysql'),
	SHA('mysql'),
	ENCODE('mysql','sajksaj')
FROM DUAL;
```

## 2.Mysql信息函数
| 函数                                                   | 说明                                                       |
| :----------------------------------------------------- | :--------------------------------------------------------- |
| VWESION()                                              | 获取当前Mysql的版本                                        |
| CONNECTION_ID()                                        | 返回当前MySQL服务器的连接数。                              |
| DATABASE(), SCHEMA()                                   | 返回MySQL命令行当前所在的数据库。                          |
| USER(), CURRENT_USER()、 SYSTEM_USER(), SESSION_USER() | 返回当前连接MySQL的用户名，返回结果格式为"主机名@用户名"。 |
| CHARSET(value)                                         | 返回字符串value自变量的字符集。                            |
| COLLATION(value)                                       | 返回字符串value的比较规则。                                |
```sql
SELECT 
	VERSION(),
	CONNECTION_ID(),
	DATABASE(),
	USER(),
	SESSION_USER(),
	CHARSET('你好'),	#utf8mb4
	COLLATION('你好')	#utf8mb4_0900_ai_ci
FROM DUAL;
```


## 3.其它函数
| 函数                           | 说明                                                                      |
| :----------------------------- | :------------------------------------------------------------------------ |
| FORMAT(value,n)                | 返回对数字value进行格式化后的结果数据。n表示四舍五入后保留到小数点后n位。 |
| CONV(value,from,to)            | 将value的值进行不同进制之间的转换。                                       |
| INET_ATON(ipvalue)             | 将以点分隔的IP地址转化为一个数字。                                        |
| INET_NTOA(value)               | 将数字形式的IP地址转化为以点分隔的IP地址。                                |
| BENCHMARK(n,expr)              | 将表达式expr重复执行n次。用于测试MySQL处理expr表达式所耗费的时间。        |
| CONVERT(value USING char_code) | 将value所使用的字符编码修改为char_code。                                  |

```sql
#其它相关函数
#n小于等于0则保留整数
SELECT
	FORMAT(123.125, 2),
	FORMAT(123.555, 1),
	FORMAT(123.555, 0)
FROM DUAL;

#进制转换 将val以某进制转到某进制进行返回
SELECT 
	CONV(16, 10, 2), 
	CONV(8888, 10, 16), 
	CONV(NULL, 10, 2)
FROM DUAL;


/*	
	IP转换
	转换原理：
		以"192.168.1.100"为例，
		计算方式为192乘以256的3次万，加上168乘以256的2次方，加上1乘以256，再加上100。
*/
SELECT 
	INET_ATON('1.12.248.253'),	#将ip转成一个整数
	INET_NTOA(INET_ATON('1.12.248.253')) #将ip整数值还原成原始ip
FROM DUAL;

#CHARSET CONVERT
SELECT 
	CHARSET('mysql'),
	CONVERT('mysql' USING 'utf8mb3'),
	CHARSET(CONVERT('mysql' USING 'utf8mb3'))
FROM DUAL
```



