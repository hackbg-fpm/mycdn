---
title: SQL随记
author: 肥胖喵
tags:
  - SQL
categories:
  - 笔记
abbrlink: b2cc
date: 2022-08-08 20:20:00
summary: 常用SQL语句及函数
---


## SQL语句及函数

### sql执行顺序

![SQL查询语句的执行顺序解析](https://imgconvert.csdnimg.cn/aHR0cDovL3AzLnBzdGF0cC5jb20vbGFyZ2UvcGdjLWltYWdlL2JmZmQ3YzkyYzIxYTQyN2VhMjBmNDkzNmY0NWI2ZDY5?x-oss-process=image/format,png)

### 联合查询：UNION

```sql
SELECT '案件总数' as `type`, SUM(case_count) as `count` 
 FROM T_ZWCX_XZSS_CASELOAD_STATISTICS
 UNION
 SELECT 
 '胜诉案件数' as `type`, SUM(success_count) as `count` 
 FROM T_ZWCX_XZSS_CASELOAD_STATISTICS
 UNION
 SELECT '败诉案件数' as `type`, SUM(loss_count) as `count`
 FROM T_ZWCX_XZSS_CASELOAD_STATISTICS
```


![](/images/pasted-10.png)

### 判空：IFNULL（）

`IFNULL(expression, alt_value)`  如果expression为空就返回 alt_value。

### 分页：Limit

页码：pageNo

页面记录数：pageSize

分页公式：limit (pageNo-1)*pageSize ,pageSize

### 字符串连接：CONCAT（exp1,exp2）

`CONCAT(exp1,exp2)` 将exp1和exp2字符串连接

### 字符串替换：REPLACE(www.baidu,'.','/')

```sql
REPLACE(www.baidu,'.','/')会将 . 替换为/
```

### 保留小数位数：**TRUNCATE(X,D)**、**round(X,2)**

- TRUNCATE(X,D)：不进行四舍五入

- round(X,2)：进行四舍五入

X 表示需要处理的数字，D 表示需要截取的位数。如果 D 为零，则返回的数字不含小数。D 也可以是负数，这样会把整数的部分置零。

```sql
  SELECT
  v1.date datetime,
  IFNULL(t.successCount, 0) "胜诉案件数",
  IFNULL(t.lossCount, 0) "败诉案件数",
	IFNULL(t.allCount, 0) "总案件数",
	CONCAT(
    TRUNCATE (
      IF (allCount != 0,(successCount / allCount) * 100, 0),
      2
    ),
    '%'
  ) AS "胜诉比例"
FROM
  year_view_recent v1
  LEFT JOIN (
    SELECT
		  SUM(case_count)  allCount,
      SUM(success_count) successCount,
      SUM(loss_count) lossCount,
      DATE_FORMAT(year, '%Y年') datetime
    FROM
      `T_ZWCX_XZSS_TYPE_STATISTICS`
    GROUP BY
      DATE_FORMAT(year, '%Y年')
  ) t ON v1.date = t.datetime
ORDER BY
  datetime
```


![](/images/pasted-11.png)





```sql
 SELECT
  v1.date datetime,
  IFNULL(t.arrearsMoney, 0) arrearsMoney,
  IFNULL(t.moneyALL, 0) moneyALL,
	CONCAT(
    TRUNCATE (
      IF (moneyALL != 0,(arrearsMoney / moneyALL) * 100, 0),
      2
    ),
    '%'
  ) AS percent
FROM
  year_month_view v1
  LEFT JOIN (
    SELECT
      SUM(arrears_money) arrearsMoney,
      SUM(project_money_count) moneyALL,
      DATE_FORMAT(create_time, '%Y年%m月') datetime
    FROM
      `T_ZWCX_ARREARS_STATISTICS`
    GROUP BY
      DATE_FORMAT(create_time, '%Y年%m月')
  ) t ON v1.date = t.datetime
ORDER BY
  datetime
```



```sql
 SELECT
  v1.date datetime,
  IFNULL(t.count, 0) count
FROM
  year_month_view v1
  LEFT JOIN (
    SELECT
      SUM(count) count,
      DATE_FORMAT(create_time, '%Y年%m月') datetime
    FROM
      `T_ZWCX_POLICY_STATISTICS`
    WHERE data_type='2'
    GROUP BY
      DATE_FORMAT(create_time, '%Y年%m月')
  ) t ON v1.date = t.datetime
ORDER BY
  datetime
```



```sql
SELECT  v1.date datetime,
  disappointCount,governCount
 from
 year_view_recent v1
 LEFT JOIN(
  SELECT name,SUM(disappoint_count) disappointCount,SUM(govern_count) governCount, DATE_FORMAT(create_time, '%Y年%m月') datetime
 FROM T_ZWCX_DISAPPOINT_GOVERN_STATISTICS
  GROUP BY
      DATE_FORMAT(create_time, '%Y年%m月')
			) t ON v1.date=t.datetime
 ORDER BY
 datetime
```

​	



```sql
 SELECT
  v1.date datetime,
  IFNULL(t.disappointCount, 0) "失信总数",IFNULL(t.governCount, 0) "治理总数"
FROM
  year_view_recent v1
  LEFT JOIN (
    SELECT 
    SUM(disappoint_count) disappointCount,SUM(govern_count) governCount,
      DATE_FORMAT(year, '%Y年') datetime
    FROM
      `T_ZWCX_DISAPPOINT_GOVERN_STATISTICS`

    GROUP BY
      DATE_FORMAT(year, '%Y年')

  ) t ON v1.date = t.datetime
ORDER BY
  datetime
```

​	

```sql
SELECT

  v1.date datetime,
  IFNULL(t.arrearsMoney, 0) "拖欠总金额",
  IFNULL(t.moneyALL, 0)  "项目总金额",
  CONCAT(
    TRUNCATE (
      IF (moneyALL != 0,(arrearsMoney / moneyALL) * 100, 0),
      2
    ),
    '%'
  ) AS "比例"
FROM
  year_month_view v1
  LEFT JOIN (
    SELECT
      SUM(arrears_money) arrearsMoney,
      SUM(project_money_count) moneyALL,
      DATE_FORMAT(create_time, '%Y年%m月') datetime
    FROM
      `T_ZWCX_ARREARS_STATISTICS`
    GROUP BY
      DATE_FORMAT(create_time, '%Y年%m月')
  ) t ON v1.date = t.datetime
ORDER BY
  datetime
```

### 判断是否存在字符：FIND_IN_SET(str,strlist)

FIND_IN_SET(str,strlist)返回strlist中str的位置，strlist不存在str则返回0。

str 要查询的字符串

strlist 字段名 参数以”,”分隔 如 (1,2,6,8,10,22)。如下：

```sql
 select find_in_set('b','a,b,c,d');//结果为2
 select find_in_set('e','a,b,c,d');//结果为0
```


![](/images/pasted-12.png)

### 类型转换：CAST (expression AS data_type) 

expression：任何有效的SQServer表达式。
AS：用于分隔两个参数，在AS之前的是要处理的数据，在AS之后是要转换的数据类型。
data_type：目标系统所提供的数据类型，包括bigint和sql_variant，不能使用用户定义的数据类型。

可以转换的类型是有限制的。这个类型可以是以下值其中的一个：

- 二进制，同带binary前缀的效果 : BINARY    
- 字符型，可带参数 : CHAR()     
- 日期 : DATE     
- 时间: TIME     
- 日期时间型 : DATETIME     
- 浮点数 : DECIMAL      
- 整数 : SIGNED     
- 无符号整数 : UNSIGNED 
  

![](/images/pasted-13.png)

### 间隔天数计算：DATEDIFF()

DATEDIFF() 函数返回两个日期之间的天数。

```sql 
SELECT DATEDIFF('2022-04-23','2022-04-24') AS DiffDate
```

结果：DiffDate = date1-date2 = 1

### 返回天数

```sql
TO_DAYS(now())   ##返回从年份0开始到now()现在的一个天数
```

### 返回当前年月日

```sql
curdate()		##获取当前的年月日
```

### 返回当前时分

```sql
curtime()		##获取当前的时分秒
```

### 返回当前日期和时间

```sql
now()			##获取当前的日期和时间
```

### 日期减去指定的时间间隔

```sql
DATE_SUB(date,INTERVAL expr type)
DATE_SUB(时间，INTERVAL 1 DAY)		##减1天
DATE_SUB(时间，INTERVAL 1 MONTH)	##减1个月
```

date 参数是合法的日期表达式

expr 参数是您希望添加的时间间隔，时间间隔参数非常全面，常用的为 年月日时分秒

### 时间差计算: TIMESTAMPDIFF()

TIMESTAMPDIFF(interval,datetime_expr1,datetime_expr2)

返回日期或日期时间表达式datetime_expr1 和datetime_expr2the 之间的整数差。其结果的单位由interval 参数给出。

interval 可以精确到年（YEAR）、天（DAY）、小时（HOUR），分钟（MINUTE）和秒（SECOND）

```sql
–相差1天
select TIMESTAMPDIFF(DAY, ‘2018-03-20 23:59:00’, ‘2015-03-22 00:00:00’);
–相差49小时
select TIMESTAMPDIFF(HOUR, ‘2018-03-20 09:00:00’, ‘2018-03-22 10:00:00’);
–相差2940分钟
select TIMESTAMPDIFF(MINUTE, ‘2018-03-20 09:00:00’, ‘2018-03-22 10:00:00’);
–相差176400秒
select TIMESTAMPDIFF(SECOND, ‘2018-03-20 09:00:00’, ‘2018-03-22 10:00:00’);
-相差1年
select TIMESTAMPDIFF(YEAR,‘2009-05-01’,‘2008-01-01’);
```

### 字符串转时间：str_to_date()

格式:str_to_date('字符串日期','日期格式');

作用:将varchar类型转换为date类型

常用的的日期格式format格式，见博文：https://blog.csdn.net/Saintmm/article/details/124432214

示例：

```sql 
SELECT STR_TO_DATE('2022/04/26','%Y/%m/%d') AS date;
```


![](/images/pasted-14.png)

### 时间转字符串：date_format()

作用:将date类型转换位varchar字符串类型,可以将数据库中的date类型的数据转换成想要的格式的字符串类型.

格式:date_format(日期类型数据,'日期格式');

在查询的时候,数据库会自动为其格式化,其默认日期格式为:%Y-%m-%d;

### 结果集指定顺序排序：field（）

```sql
order by field(str，str1，str2，str3，str4……)
```

字段str按照字符串str1，str2，str3，str4的**顺序**返回查询到的结果集。如果表中str字段值不存在于str1，str2，str3，str4中的记录，放在结果集最前面返回。

### 等值转换、范围转换、列转行：case when

#### 1. 等值转换

示例：

```sql
select
name as '名字',
(case sex 
	when 0 then '女' 
 	else '男' end) as '性别'
from student;
```


![](/images/pasted-15.png)

#### 2. 范围转换

**注意：**此处case后面为when

示例：

```sql
select 
name as '姓名',
(case  when score>=90 then '优' 
 	   when score>=80 then '良' 
 	   when score>=60 then '及格' 
       else '不及格' end) as '等级'
from stu_score; 
```


![](/images/pasted-16.png)

#### 3. 列转行

示例：

将图一转换成图二

![](/images/pasted-17.png)

1.  先按照科目分开， 符合条件的设置分数，不符合的给置零。

```sql
select name as '姓名'
,(case course when '语文' then score else 0 end) as '语文'
,(case course when '数学' then score else 0 end) as '数学'
,(case course when '英语' then score else 0 end) as '英语'
from test.course_score
```


![](/images/pasted-18.png)

2. 然后再按照名字group by ，对分数求max。

```sql
select name as '姓名'
,max(case course when '语文' then score else 0 end) as '语文'
,max(case course when '数学' then score else 0 end) as '数学'
,max(case course when '英语' then score else 0 end) as '英语'
from test.course_score group by name;
```


![](/images/pasted-19.png)

### 数据筛选：HAVING

HAVING一般和group by联合使用

HAVING对分组后的数据进行筛选

where和having优先选择使用where

### 插入数据如果存在则更新：ON DUPLICATE KEY UPDATE 

存在即更新，如果新增的数据中主键或唯一索引存在则执行更新操作，否则执行新增操作。

```sql
insert into user
(id,username,age)
values
(#{id},#{username},#{age})
on duplicate key update
username=values(username),
age=values(age)
```

当新增数据的主键id在现有表中存在时就执行update语句。

### MySQL中实现rank排名查询

https://blog.csdn.net/justry_deng/article/details/80597916?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522165465502216781667894765%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=165465502216781667894765&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-1-80597916-null-null.142^v11^pc_search_result_control_group,157^v13^control&utm_term=%40rank&spm=1018.2226.3001.4187

### MySQL批量修改

https://blog.csdn.net/justry_deng/article/details/83064354

 ### ALTER 语句修改数据表

1.修改数据表名：alter table 表名 rename 新表名;

2.修改列名： alter table 表名 change 列名 新列名（可以与旧的一样） 类型 默认值;

3.修改类型： alter table 表名 modify 列名 类型

4.修改默认值： alter table 表名 alter column 字段名 drop default; alter table 表名 alter column 字段名 set default 默认值;

5.插入列：alter table 表名 add 列名 类型;

6.删除列：alter table 表名drop 列名;

7.修改引擎：alter table 表名 engine=myisam;

8.删除外键约束：alter talbe 表名 drop foreign key 名

9.修改某一列为自增：alter table test change id id int AUTO_INCREMENT;

### 分组连接GROUP_CONCAT()

GROUP_CONCAT(),   tagName为连接tag连接后的字段值

```java
		SELECT type1,type2,a.product_name,b.product_name productName2,ent_name,invest,tag ,
				GROUP_CONCAT(DISTINCT c.tag_name ORDER BY find_in_set(c.id,t.tag)) tagName
				FROM zy_product_map t
				LEFT JOIN semi_product_node_count a ON t.type1=a.id
				LEFT JOIN semi_product_node_count b ON t.type2=b.id
				LEFT JOIN semi_tag_ccid c ON find_in_set(c.id, t.tag) > 0 
				GROUP BY t.id
```


![](/images/pasted-20.png)

## 视图

### 当前年的一月到十二月

```sql
SELECT
	concat( date_format( curdate(), '%Y' ), '年01月' ) AS `date` UNION
SELECT
	concat( date_format( curdate(), '%Y' ), '年02月' ) AS `date` UNION
SELECT
	concat( date_format( curdate(), '%Y' ), '年03月' ) AS `date` UNION
SELECT
	concat( date_format( curdate(), '%Y' ), '年04月' ) AS `date` UNION
SELECT
	concat( date_format( curdate(), '%Y' ), '年05月' ) AS `date` UNION
SELECT
	concat( date_format( curdate(), '%Y' ), '年06月' ) AS `date` UNION
SELECT
	concat( date_format( curdate(), '%Y' ), '年07月' ) AS `date` UNION
SELECT
	concat( date_format( curdate(), '%Y' ), '年08月' ) AS `date` UNION
SELECT
	concat( date_format( curdate(), '%Y' ), '年09月' ) AS `date` UNION
SELECT
	concat( date_format( curdate(), '%Y' ), '年10月' ) AS `date` UNION
SELECT
	concat( date_format( curdate(), '%Y' ), '年11月' ) AS `date` UNION
SELECT
	concat( date_format( curdate(), '%Y' ), '年12月' ) AS `date`
```

### 当前年月及前十一个月

```sql
SELECT
	date_format( curdate(), '%Y-%m' ) AS `date` UNION
SELECT
	date_format(( curdate() - INTERVAL 1 MONTH ), '%Y-%m' ) AS `date` UNION
SELECT
	date_format(( curdate() - INTERVAL 2 MONTH ), '%Y-%m' ) AS `date` UNION
SELECT
	date_format(( curdate() - INTERVAL 3 MONTH ), '%Y-%m' ) AS `date` UNION
SELECT
	date_format(( curdate() - INTERVAL 4 MONTH ), '%Y-%m' ) AS `date` UNION
SELECT
	date_format(( curdate() - INTERVAL 5 MONTH ), '%Y-%m' ) AS `date` UNION
SELECT
	date_format(( curdate() - INTERVAL 6 MONTH ), '%Y-%m' ) AS `date` UNION
SELECT
	date_format(( curdate() - INTERVAL 7 MONTH ), '%Y-%m' ) AS `date` UNION
SELECT
	date_format(( curdate() - INTERVAL 8 MONTH ), '%Y-%m' ) AS `date` UNION
SELECT
	date_format(( curdate() - INTERVAL 9 MONTH ), '%Y-%m' ) AS `date` UNION
SELECT
	date_format(( curdate() - INTERVAL 10 MONTH ), '%Y-%m' ) AS `date` UNION
SELECT
	date_format(( curdate() - INTERVAL 11 MONTH ), '%Y-%m' ) AS `date`
```

### 最近六年

```sql
SELECT
	date_format( curdate(), '%Y年' ) AS `date` UNION
SELECT
	date_format(( curdate() - INTERVAL 1 YEAR ), '%Y年' ) AS `date` UNION
SELECT
	date_format(( curdate() - INTERVAL 2 YEAR ), '%Y年' ) AS `date` UNION
SELECT
	date_format(( curdate() - INTERVAL 3 YEAR ), '%Y年' ) AS `date` UNION
SELECT
	date_format(( curdate() - INTERVAL 4 YEAR ), '%Y年' ) AS `date` UNION
SELECT
	date_format(( curdate() - INTERVAL 5 YEAR ), '%Y年' ) AS `date`
```



 ## 时间相关SQL语句

### 查询5分钟前的数据

```sql
select * from table where end_date between date_add(now(), interval - 300 SECOND) and NOW() 
```

### 查询当天的所有数据

```sql
SELECT * FROM 表名 WHERE DATEDIFF(字段,NOW())=0;

select * from 表名 where to_days(时间字段名) = to_days(now());

SELECT * FROM 表名 WHERE date(时间字段名) = curdate();
```

### 查询昨天的所有数据

```sql
SELECT * FROM 表名 WHERE DATEDIFF(字段,NOW())=-1
```

### 查询未来第n天的所有数据

```sql
//当n为负数时，表示过去第n天的数据
SELECT * FROM 表名WHERE DATEDIFF(字段,NOW())=n
```

### 查询未来n天内所有数据

```sql
SELECT * FROM 表名 WHERE DATEDIFF(字段,NOW())<n AND DATEDIFF(字段,NOW())>=0
```

### 查询过去n天内所有数据

```sql
//包含当天
SELECT * FROM 表名 WHERE DATEDIFF(字段,NOW())<=0 AND DATEDIFF(字段,NOW())>-n

//不包含当天
SELECT * FROM 表名 WHERE DATEDIFF(字段,NOW())<0 AND DATEDIFF(字段,NOW())>-n
```

### 查询昨天所有数据

```sql
SELECT * FROM 表名 WHERE TO_DAYS( NOW( ) ) - TO_DAYS( 时间字段名) <= 1
```

### 查询近7天所有数据

```sql
SELECT * FROM 表名 where DATE_SUB(CURDATE(), INTERVAL 7 DAY) <= date(时间字段名)
```

### 查询近30天所有数据

```sql
SELECT * FROM 表名 where DATE_SUB(CURDATE(), INTERVAL 30 DAY) <= date(时间字段名)
```

### 查询本月所有数据

```sql
SELECT * FROM 表名 WHERE DATE_FORMAT( 时间字段名, '%Y%m' ) = DATE_FORMAT( CURDATE( ) , '%Y%m' )
```

### 查询上月所有数据

```sql
SELECT * FROM 表名 WHERE PERIOD_DIFF( date_format( now( ) , '%Y%m' ) ,date_format( 时间字段名, '%Y%m' ) ) =1
```

### 查询本季度所有数据

```sql
select * from `ht_invoice_information` where QUARTER(create_date)=QUARTER(now());
```

### 查询上季度所有数据

```sql
select * from `ht_invoice_information` where QUARTER(create_date)=QUARTER(DATE_SUB(now(),interval 1 QUARTER));
```

### 查询本年所有数据

```sql
select * from `ht_invoice_information` where YEAR(create_date)=YEAR(NOW());
```

### 查询上年所有数据

```sql
select * from `ht_invoice_information` where year(create_date)=year(date_sub(now(),interval 1 year));
```

### 查询当前这周的所有数据

```sql
SELECT name,submittime FROMenterprise WHERE YEARWEEK(date_format(submittime,'%Y-%m-%d')) = YEARWEEK(now());
```

### 查询上周的所有数据

```sql
SELECT name,submittime FROMenterprise WHERE YEARWEEK(date_format(submittime,'%Y-%m-%d')) = YEARWEEK(now())-1;
```

### 查询上个月的数据

 ```sql
 select name,submittime fromenterprise where date_format(submittime,'%Y-%m')=date_format(DATE_SUB(curdate(), INTERVAL 1 MONTH),'%Y-%m')
 
 select * from user where DATE_FORMAT(pudate,'%Y%m') = DATE_FORMAT(CURDATE(),'%Y%m') ;
 
 select * from user where WEEKOFYEAR(FROM_UNIXTIME(pudate,'%y-%m-%d')) = WEEKOFYEAR(now())
 
 select * from user where MONTH(FROM_UNIXTIME(pudate,'%y-%m-%d')) = MONTH(now())
 
 select * from user where YEAR(FROM_UNIXTIME(pudate,'%y-%m-%d')) = YEAR(now()) and MONTH(FROM_UNIXTIME(pudate,'%y-%m-%d')) = MONTH(now()) 
 ```

### 查询当前月份的数据 

```sql
select name,submittime fromenterprise  wheredate_format(submittime,'%Y-%m')=date_format(now(),'%Y-%m')
```

### 查询距离当前现在6个月的数据

```sql
select name,submittime fromenterprise where submittime betweendate_sub(now(),interval 6 month) and now();
```

### 更新表中有效期valid_time字段值都增加一天

```sql
UPDATE cqh_activity SET valid_time=DATE_ADD(valid_time,INTERVAL 1 DAY);
```

### 更新表中有效期valid_time字段值都减少一天

```sql
UPDATE cqh_activity SET valid_time=DATE_SUB(valid_time,INTERVAL 1 DAY);
```








​	


​	
