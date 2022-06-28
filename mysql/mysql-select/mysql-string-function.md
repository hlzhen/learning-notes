# Mysql字符串函数

## 1.字符串函数
| 函数                           | 说明                                                                                                          |
| :----------------------------- | :------------------------------------------------------------------------------------------------------------ |
| ASCII(s)                       | 返回字符串s中的第一个字符的ASCII码值。                                                                        |
| CHAR_LENGTH(s)                 | 返回字符串s的{% em color="#EEEED1" %}字符数{% endem %}。与CHARACTER_LENGTH(s)相同。                           |
| LENGTH(s)                      | 返回字符串s的{% em color="#EEEED1" %}字节数{% endem %}，和字符集有关。                                        |
| CONCAT(s1,s2,...,sn)           | 连接s1,s2,....,sn为一个字符串。如s1~sn 中有null值则结果为null                                                 |
| CONCAT_WS(x,s1,s2,...,sn)      | 连接s1,s2,....,sn为一个字符串，其以x为分割符。如x为null则结果为null。                                         |
| INSERT(str,idx,len,replacestr) | 将字符串str从第idx位置开始，len个字符长的子串替换为字符串replacestr                                           |
| REPLACE(str,a,b)               | 用字符串b替换字符串str中所有出现的字符串a。                                                                   |
| UPPER(s)或UCASE(s)             | 将字符串s的所有字母转换成{% em color="#EEEED1" %}大写{% endem %}字母。                                        |
| LOWER(s)或LCASE(s)             | 将字符串s的所有字母转换成{% em color="#EEEED1" %}小写{% endem %}字母。                                        |
| LEFT(str,len)                  | 返回字符串str最左边的len个字符。                                                                              |
| RIGHT(str,len)                 | 返回字符串str最右边的len个字符。                                                                              |
| LPAD(str,len,pad)              | 用字符串pad对str最左边进行填充,直到str的长度为len个字符,len需大于str的长度才会进行填充。                      |
| RPAD(str,len,pad)              | 用字符串pad对str最右边进行填充,直到str的长度为len个字符,len需大于str的长度才会进行填充。                      |
| LTRIM(s)                       | 去掉字符串s左侧的空格。                                                                                       |
| RTRIM(s)                       | 去掉字符串s右侧的空格。                                                                                       |
| TRIM(s)                        | 去掉字符串s开始与结尾的空格。                                                                                 |
| TRIM(s1 from s)                | 去掉字符串s开始与结尾的s1。                                                                                   |
| TRIM(LEADING s1 from s)        | 去掉字符串s开始处的s1。                                                                                       |
| TRIM(TRAILING s1 from s)       | 去掉字符串s结尾处的s1。                                                                                       |
| REPEAT(str,n)                  | 返回str重复n次的结果。                                                                                        |
| SPACE(n)                       | 返回n个空格。                                                                                                 |
| STRCMP(s1,s2)                  | 比较字符串s1，s2的ASCII码值的大小。                                                                           |
| SUBSTR(s,idx,len)              | 返回从字符串s的idx位置其len个字符，作用与SUBSTRING(s,n,len)、MID(s,n，len)相同。                              |
| LOCATE(substr,str)             | 返回字符串substr在字符串str中首次出现的位置，与POSITION(substr in str)/INSTR(str,substr)相同。未找到，返回0。 |
| ELT(m,s1,s2,...,sn)            | 返回指定位置的字符串，如果m=1，则返回s1,如果m=2,则返回s2 ，如果m=n，则返回sn                                  |
| FIELD(s,s1,s2,...,sn)          | 返回字符串s在字符串列表中第一次出现的位置                                                                     |
| FIND_IN_SET(s1,s2)             | 返回字符串s1在字符串s2中出现的位置。其中，字符串s2是一个以逗号分隔的字符串                                    |
| REVERSE(s)                     | 返回s反转后的字符串                                                                                           |
| NULLIF(value1,value2)          | 比较两个字符串，如果value1与value2相等，则返回NULL，否则返回value1                                            |

```sql
#字符串函数
SELECT
	ASCII('z'),
	CHAR_LENGTH('abc你好呀'),
	LENGTH('abc你好呀'),
	CONCAT('你', '好', '呀'),
	CONCAT_WS('-', '你', '好', '呀'),
	INSERT('Mysql_hello_world',3,3,'SQL'),
	REPLACE('Mysql_hello_world','_','***'),
	UPPER('你好mysql'),
	UCASE('你好Mysql'),
	LOWER('你好MYSQL'),
	LOWER('你好MySQL'),
	LEFT('你好MYSÏQL',2),
	RIGHT('你好MYSQL',5),
	LPAD('MYSQL',6,'*'),
	RPAD('mysql',7,'*')
FROM DUAL;
```