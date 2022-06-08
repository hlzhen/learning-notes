# Mysql日期时间函数

## 1.获取日期与时间
| 函数                        | 说明                             |
| :-------------------------- | :------------------------------- |
| CURDATE() 或 CURRENT_DATE() | 返回当前日期，只包含年、月、日。 |
| CURTIME() 或 CURRENT_TIME() | 返回当前时间，只包含时、分、秒。 |
| NOW()                       | 返回当前系统日期和时间。         |
| SYSDATE()                   | 返回当前系统日期和时间。         |
| CURRENT_TIMESTAMP()         | 返回当前系统日期和时间。         |
| LOCALTIME()                 | 返回当前系统日期和时间。         |
| LOCALTIMESTAMP()            | 返回当前系统日期和时间。         |
| UTC_DATE()                  | 返回UTC(世界标准时间)日期        |
| UTC_TIME()                  | 返回UTC(世界标准时间)时间。      |
```sql
#返回当前日期，只包含年、月、日。
SELECT CURDATE(), CURRENT_DATE() FROM DUAL;

#返回当前时间，只包含时、分、秒。
SELECT CURTIME(), CURRENT_TIME() FROM DUAL;

#返回当前系统日期和时间。
SELECT 
	NOW(),
	SYSDATE(),
	CURRENT_TIMESTAMP(),
	LOCALTIME(),
	LOCALTIMESTAMP()
FROM DUAL;

#返回UTC(世界标准时间)日期与时间
SELECT UTC_DATE(), UTC_TIME() FROM DUAL;
```

## 2.日期与时间戳的转换
| 函数                     | 说明                                                                   |
| :----------------------- | :--------------------------------------------------------------------- |
| UNIX_TIMESTAMP()         | 以UNIX时间戳的形式返回当前时间。SELECT UNIX_TIMESTAMP() ->1634348884。 |
| UNIX_TIMESTAMP(date)     | 将时间date以unix时间戳的形式返回。                                     |
| FROM_UNIXTIME(timestamp) | 将unix时间戳的时间转换为普通格式的时间返回。                           |
```sql
#返回UTC(世界标准时间)日期与时间
SELECT UTC_DATE(), UTC_TIME() FROM DUAL;

#以UNIX时间戳的形式返回当前时间。SELECT UNIX_TIMESTAMP() ->1634348884。
SELECT UNIX_TIMESTAMP() FROM DUAL;

#将时间date以unix时间戳的形式返回。
SELECT UNIX_TIMESTAMP('2019-05-07 15:59:22') FROM DUAL;

#将unix时间戳的时间转换为普通格式的时间返回。
SELECT FROM_UNIXTIME(UNIX_TIMESTAMP('2019-05-07 15:59:22')) FROM DUAL;
```

## 3.获取月份、星期、星期数、天数等函数
| 函数                                 | 说明                                     |
| :----------------------------------- | :--------------------------------------- |
| YEAR(date)/MONTH(date)/DAY(date)     | 返回具体的日期值。                       |
| HOUR(time)/MINUTE(time)/SECOND(time) | 返回具体的时间值。                       |
| MONTHNAME(date)                      | 返回月份: January，...                   |
| DAYNAME(date)                        | 返回星期几: MONDAY，TUESDAY，SUNDAY，... |
| WEEKDAY(date)                        | 返回周几: 周一为0，周二为1....周日为6    |
| QUARTER(date)                        | 返回日期对应的季度，范围1～4             |
| WEEK(date)/WEEKOFYEAR(date)          | 返回一年中的第几周                       |
| DAYOFYEAR(date)                      | 返回日期是一年中的第几天                 |
| DAYOFMONTH(date)                     | 返回日期位于所在月份的第几天             |
| DAYOFWEEK(date)                      | 返回周几，周日是1，周一是2，...周六是7   |
```sql
#返回具体的日期值。
SELECT  YEAR(NOW()), MONTH(NOW()), DAY(NOW()) FROM DUAL;

#返回具体的时间值。
SELECT HOUR(CURTIME()), MINUTE(NOW()), SECOND( SYSDATE()) FROM DUAL;


SELECT 
	MONTHNAME('2021-10-26'),
	DAYNAME('2021-10-26'),
	WEEKDAY('2021-10-26'),
	QUARTER(CURDATE()),
	WEEK(CURDATE()),
	DAYOFYEAR(NOW()), 
	DAYOFMONTH(NOW()), 
	DAYOFWEEK(NOW())	
FROM DUAL;
```

## 4.日期的操作函数
| 函数                    | 说明                                         |
| :---------------------- | :------------------------------------------- |
| EXTRACT(type from date) | 返回指定日期中特定的部分，type指定返回的值。 |
```markdown
# type的可取值
`MICROSECOND`           : 返回毫秒数。
`SECOND`                : 返回秒数。
`MINUTE`                : 返回分钟数
`HOUR`                  : 返回小时数 
`DAY`                   : 返回天数
`WEEK`                  : 返回日期在一年中的第几个星期
`MONTH`                 : 返回日期在一年中的第几个月
`QUARTER`               : 返回日期在一年中的第儿个季度
`YEAR`                  : 返回日期的年份
`SECOND_MICROSECOND`    : 返回秒和毫秒值
`MINUTE_MICROSECOND`    : 返回分钟和亳秒值
`MINUTE_SECOND`         : 返回分钟和秒值
`HOUR_MICROSECOND`      : 返回小时和毫秒值
`HOUR_SECOND`           : 返回小时和秒值
`HOUR_MINUTE`           : 返回小时和分钟值
`DAY_MINUTE`            : 返回天和毫秒值
`DAY_SECOND`            : 返回天和秒值
`DAY_MINUTE`            : 返回天和分钟值
`YEAR_MONTH`            : 返回年和月
```
```sql
#日期的操作函数
SELECT 
	EXTRACT(MINUTE FROM NOW()),
	EXTRACT(WEEK FROM NOW()), 
	EXTRACT(QUARTER FROM NOW()),
	EXTRACT(MINUTE_SECOND FROM NOW()) 
FROM DUAL;
```


## 5.时间和秒钟转换的函数
| 函数                 | 说明                                                                     |
| :------------------- | :----------------------------------------------------------------------- |
| TIME_TO_SEC(time)    | 将time转化为秒并返回结果值。转化的公式为：`小时 * 3600 + 分钟 * 60 + 秒` |
| SEC_TO_TIME(seconds) | 将seconds描述转化为包含小时、分钟和秒的时间。                            |
```sql
#时间和秒钟转换的函数
SELECT 
	TIME_TO_SEC(NOW()), 
	SEC_TO_TIME(57143)
FROM DUAL;
```

## 6.计算日期和时间的函数
### 6.1 函数分类1
| 函数                                  | 说明                                           |
| :------------------------------------ | :--------------------------------------------- |
| DATE_ADD(datetime,INTERVAL expr type) | 返回与给定日期时间相差INTERVAL时间段的日期时间 |
| ADDDATE(date,INTERVAL expr type)      | 返回与给定日期时间相差INTERVAL时间段的日期时间 |
| DATE_SUB(date, INTERVAL expr type)    | 返回与date相差INTERVAL时间间隔的日期。         |
| SUBDATE(date, INTERVAL expr type)     | 返回与date相差INTERVAL时间间隔的日期。         |
```markdown
# type的取值
`HOUR`          : 小时
`MINUTE`        : 分钟
`SECOND`        : 秒
`YEAR`          ：年
`MONTH`         ：月
`DAY`           : 日
`YEAR_MONTH`    : 年和月
`DAY_HOUR`      : 日和小时
`DAY_MINUTE`    : 日和分钟
`DAY_SECOND`    : 日和秒
`HOUR_MINUTE`   : 小时和分钟
`HOUR_SECOND`   : 小时和秒
`MINUTE_SECOND` : 分钟和秒
```
```sql
#计算日期和时间的函数，例1:
SELECT 
	NOW(),
	DATE_ADD(NOW(),INTERVAL 1 YEAR),
	DATE_ADD(NOW(),INTERVAL -1 YEAR),
	DATE_SUB(NOW(),INTERVAL 1 YEAR)
	DATE_SUB(NOW(),INTERVAL -1 YEAR),
FROM DUAL;

#计算日期和时间的函数，例2:
SELECT 
	DATE_ADD(NOW(), INTERVAL 1 DAY) AS col1, 
	DATE_ADD('2021-10-21 23:32:12' ,INTERVAL 1 SECOND) AS col2, 
	ADDDATE('2021-10-21 23:32:12',INTERVAL 1 SECOND) AS col3, 
	DATE_ADD('2021-10-21 23:32:12' , INTERVAL "1_1" MINUTE_SECOND) AS col4, #分钟与秒数各加1
	DATE_ADD(NOW(), INTERVAL -1 YEAR) AS col5, #可以是负数 
	DATE_ADD(NOW(), INTERVAL '1_1' YEAR_MONTH) AS col6 #需要单引号 年与月各加1
FROM DUAL;
```

### 6.2 函数分类2
| 函数                         | 说明                                                                    |
| :--------------------------- | :---------------------------------------------------------------------- |
| ADDTIME(time1,time2)         | 返回time1加上time2的时间。当time2为一个数字时，代表的是秒，可以为负数   |
| SUBTIME(time1,time2)         | 返回time1减去time2后的时间。当time2为一个数字时，代表的是秒，可以为负数 |
| DATEDIFF(datei,date2)        | 返回date1 与 date2的日期间隔天数。                                      |
| TIMEDIFF(time1,time2)        | 返回time1 与 time2的时间间隔。                                          |
| FROM_DAYS(N)                 | 返回从0000年1月1日起，N天以后的日期                                     |
| TO_DAYS(date)                | 返回日期date距离0000年1月1日的天数。                                    |
| LAST_DAY(date)               | 返回date所在月份的最后一天的日期。                                      |
| MAKEDATE(year,n)             | 针对给定年份与所在年份中的天数返回一个日期。                            |
| MAKETIME(hour,minute,second) | 将给定的小时、分钟和秒组合成时间并返回。                                |
| PERIOD_ADD(time,n)           | 返回time加上n后的时间。                                                 |
```sql
#计算日期和时间的函数
SELECT 
	ADDTIME(NOW(), 20), 
	SUBTIME(NOW(), 30), 
	SUBTIME(NOW(), '1:1:3'), 
	DATEDIFF(NOW(), '2021-10-01'), 
	TIMEDIFF(NOW(), '2021-10-25 22:18:10'), 
	FROM_DAYS(366), 
	TO_DAYS('0000-12-25'), 
	LAST_DAY(NOW()), 
	MAKEDATE(YEAR(NOW()), 12), 
	MAKETIME(10,21,23),
	PERIOD_ADD(20200101010101,10)
FROM DUAL;
```

## 7.日期的格式化与解析
| 函数                              | 说明                                         |
| :-------------------------------- | :------------------------------------------- |
| DATE_FORMAT(date,fmt)             | 按照字符串fmt格式化日期date值。              |
| TIME_FORMAT(time,fmt)             | 按照字符串fmt格式化日期time值。              |
| GET_FORMAT(date_type,format_type) | 返回日期字符串的显示格式。                   |
| STR_TO_DATE(str,fmt)              | 按照字符串fmt对str进行解析，解析为一个日期。 |
```markdown
# 格式符号说明(不包含GET_FORMAT函数中的格式)
`%Y`		: 4位数字表示年份
`%y`		: 表示两位数字表示年份
`%M`		: 月名表示月份(January...)
`%m`		: 两位数字表示月份(01，02，03...)
`%b`		: 缩写的月名(Jan,Feb,...)
`%c`		: 数字表示月份(1，2，3...)
`%D`		: 英文后缀表示月中的天数 (1st，2nd，3rd...)
`%d`		: 两位数字表示月中的天数(01，02...)
`%e`		: 数字形式表示月中的天数(1，2，3，4，5...)
`%H`		: 两位数字表示小数，24小时制(01，02...)
`%h或%I`	: 两位数字表示小时，12小时制(01，02...)
`%k`		: 数字形式的小时，24小时制(1，2，3)
`%l`		: 数字形式表示小时，12小时制(1，2，3...)
`%i`		: 两位数字表示分钟(00，01，02)
`%S或%s`	: 两位数字表示秒(00，01，02..)
`%W`		: 一周中的星期名称(Sunday..)
`%w`		: 以数字表示周中的天数(0=Sunday,1=Monday...)
`%a`		: 一周中的星期缩写(Sun., Mon., Tues., .)
`%j`		: 以3位数字表示年中的天数(001，002.)
`%U`		: 以数字表示年中的第几周，(1，2，3..)其中Sunday为周中第一天
`%u`		: 以数字表示年中的第几周，(1，2，3..)其中Manday为周中第一天
`%T`		: 24小时制
`%r`		: 12小时制
`%p`		: AM或PM
`%%`		: 表示%

# GET_FORMAT格式符号说明
date_type			format_type				返回值
`DATE`				`USA`					`%m.%d.%Y`
`DATE`				`JIS`					`%Y-%m-%d`
`DATE`				`ISO`					`%Y-%m-%d`
`DATE`				`EUR`					`%d.%m.%Y`
`DATE`				`INTERNAL`				`%Y%m%d`
`TIME`				`USA`					`%h:%i:%s %p`
`TIME`				`JIS`					`%H:%i:%s`
`TIME`				`ISO`					`%H:%i:%s`
`TIME`				`EUR`					`%H.%i.%s`
`TIME`				`INTERNAL`				`%H%i%s`
`DATETIME`			`USA`					`%Y-%m-%d %H.%i.%s`
`DATETIME`			`JIS`					`%Y-%m-%d %H:%i:%s`
`DATETIME`			`ISO`					`%Y-%m-%d %H:%i:%s`
`DATETIME`			`EUR`					`%Y-%m-%d %H.%i.%s`
`DATETIME`			`INTERNAL`				`%Y%m%d%H%i%s`
```
```sql
#日期格式化与解析案例
SELECT 
	DATE_FORMAT(NOW(),'%Y-%M-%D'),
	DATE_FORMAT(NOW(),'%Y-%m-%d'),
	DATE_FORMAT(NOW(),'%Y-%m-%d %I:%i:%s'), 	
	DATE_FORMAT(NOW(),'%Y-%m-%d %H:%i:%s'),
	DATE_FORMAT(NOW(),'%Y-%m-%d %l:%i:%s'),
	DATE_FORMAT(NOW(),'%Y-%m-%d %k:%i:%s'),
	DATE_FORMAT(NOW(),'%Y-%m-%d %H:%i:%s %W %w %j %u %p %T'),
	STR_TO_DATE('2022-06-08 10:34:16 Wednesday 3 159 23','%Y-%m-%d %H:%i:%s %W %w %j %u')
FROM DUAL;

SELECT 
	DATE_FORMAT(NOW(), GET_FORMAT(DATE, 'USA')),
	DATE_FORMAT(NOW(), GET_FORMAT(DATE, 'ISO')),
	DATE_FORMAT(NOW(), GET_FORMAT(TIME, 'USA')),
	DATE_FORMAT(NOW(), GET_FORMAT(TIME, 'ISO')),
	DATE_FORMAT(NOW(), GET_FORMAT(DATETIME, 'USA')),
	DATE_FORMAT(NOW(), GET_FORMAT(DATETIME, 'ISO'))
FROM DUAL;
```
