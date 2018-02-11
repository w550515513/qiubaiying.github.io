---
layout:     post
title:        PyMySql的使用
subtitle:   python与mysql初步总结
date:       2018-02-03
author:    WSS
header-img: img/post-bg-os-metro.jpg
catalog: true
tags:
    - Mysql
    - Python
---


## MySql的python包 ##

名字`mysqlclient`

建议手动安装，下载位置:[https://pypi.python.org/pypi/mysqlclient/1.3.12](https://pypi.python.org/pypi/mysqlclient/1.3.12)，查找适合自己电脑与python版本的轮子

使用cmd定位到路径

`pip install packagename(wholename)`


## Connect MySql ##


```python
import MySQLdb

# 打开数据库连接eg:(host='localhost', user='root', password='888888', db='tianqi', charset='utf8')
conn = MySQLdb.connect("localhost","testuser","test123","TESTDB" )
conn.set_character_set('utf8')
# 使用cursor()方法获取操作游标 
cur = conn.cursor()
```

## Create Table ##

可通过两种方式进行创建表：
1、MySql界面操作
2、MySql代码操作

```python
sql = """CREATE TABLE EMPLOYEE (
         FIRST_NAME  CHAR(20) NOT NULL,
         LAST_NAME  CHAR(20),
         AGE INT,  
         SEX CHAR(1),
         INCOME FLOAT )"""

cursor.execute(sql)

cur.close()
conn.commit()
conn.close()
```
sql写完后必须要提交commit()，尤其是insert, update, delete时，否则数据库没有变化!!! 
而像select这种普通的查询，不涉及修改数据库的，是否commit()没有关系

若有必要，则还需`conn.rollback()`  取消当前事务，可采用如下写法
```python
try:
   cur.execute(sql)
   db.commit()
except:
   # 发生错误时回滚
   db.rollback()
```

## Insert Data ##

`table_name eg: tianqi.weather`

mysql的数据格式主要包括5类：[data type of mysql](http://blog.csdn.net/bzhxuexi/article/details/43700435)

```python
conn = MySQLdb.connect("localhost","testuser","test123","TESTDB" )
conn.set_character_set('utf8')
# 使用cursor()方法获取操作游标 
cur = conn.cursor()
#方式一：
cur.execute("INSERT INTO table_name (column_name1, column_name2 ,column_name3 ,column_name4) 
						VALUES (%s, %s, %s, %s)",(value1, value2,value3, value4))
						
#方式二：
sql = "INSERT INTO ip (ip ,port ,response_time ,test_time) VALUES ('%s', '%s', '%f', '%d')" %(
                str(item_dict["IP"]), str(item_dict["port"]), item_dict["response_time"], item_dict["update_time"])

            print(sql)
            cur.execute(sql)

cur.close()
conn.commit()
conn.close()					
```


## Select Data ##

>**★**[参考资料](http://bbs.csdn.net/topics/390407669),[参考资料](http://blog.csdn.net/changjiangbuxi/article/details/13169861)

`SELECT column_name,column_name FROM table_name [WHERE Clause] [LIMIT N][ OFFSET M]`

>查询语句中FROM后可以使用一个或者多个表，表之间使用逗号(,)分割，并使用WHERE语句来设定查询条件。

>"*"代替其他字段，SELECT语句会返回表的所有字段数据，即满足条件的一行数据。否则只显示所选列数据。

>可以使用 WHERE 语句来包含任何条件，可用and,or连接。

>可以使用 LIMIT 属性来设定返回的记录数。

>可以通过OFFSET指定SELECT语句开始查询的数据偏移量。默认情况下偏移量为0。

#### 选择所有数据 ####
```python
cur.execute("select * from table_name")
#fetchone() 返回一条结果行,fetchall(self)        匹配所有剩余结果
results = cursor.fetchall()#元组型数据
results = list(results)
```

#### 选择某列数据 ####
```python
cur.execute("select column_name1,column_name2 from table_name")
results = cursor.fetchall()#元组型数据
results = list(results)
```

#### 查询数值型数据 ####
```python
#数值查询
cur.execute("SELECT * FROM table_name WHERE sum > 100")
cur.execute("SELECT * FROM table_name WHERE sum > 100 and sum2 <100")
cur.execute("SELECT * FROM table_name WHERE sum > 100 or sum2 <100")
cur.execute("SELECT * FROM tianqi.user_agent where id [not] between 2 and 4")#2,3,4三个
cur.execute("SELECT * FROM tianqi.user_agent where id [not] in (2,4)")#2,3,4三个

#数值计算查询
cur.execute("SELECT * FROM table_name where column_name1-column_name2 = 2")
cur.execute("SELECT * FROM table_name where column_name1 = column_name2+2")


#字符串查询
cur.execute("SELECT * FROM table_name WHERE times = "2018年2月更新"")
cur.execute("SELECT * FROM table_name WHERE times like "%2018年%"")#包含“2018年”字符的数据
cur.execute("SELECT * FROM table_name WHERE times like "a%b"")#以a开头以b结尾的字符串的数据
cur.execute("SELECT * FROM table_name WHERE times like "m_n"")#以m开头以n结尾的3个字符的数据，一个_一个字符

#查询日期型数据
cur.execute("SELECT * FROM table_name WHERE date != '2011-04-08'")#比较日期时，“<”表示早于，“>”表示晚于。

results = cursor.fetchall()#元组型数据
results = list(results)
```

#### 查询非空数据 ####
```python
#查询某列中的非空数据
cur.execute("SELECT * FROM table_name where column_name is not null")

results = cursor.fetchall()#元组型数据
results = list(results)
```

#### 查询前2条记录 ####
```python
cur.execute("SELECT * FROM table_name limit 2")
```

#### 配合order by 查询数据 ####
```python
#desc降序，asc升序，先排序后取前几
cur.execute("SELECT * FROM table_name order by id desc[asc] limit 10")
```

#### GROUP BY关键字分组查询 ####

```python
#用GROUP BY关键字分组查询,根据column_name1进行分组
cur.execute("select column_name1,column_name2 from table_name GROUP BY column_name1")
#根据两个列进行分组，只有两个数据都不同是才分为两组
cur.execute("SELECT * FROM table_name GROUP BY column_name1,column_name2")
#分组查询配合GROUP_CONCAT()来使用，可以看到每个组中的详细信息
#可以看到每个分组中都包括哪些username
cur.execute(" SELECT *,GROUP_CONCAT(username) FROM table_name GROUP BY column_name1")
```
>**★**[group by 参考资料](http://blog.csdn.net/lingyun_blog/article/details/44099783),[group by 参考资料](http://wiki.jikexueyuan.com/project/mysql/useful-functions/group.html)

#### DISTINCT关键字去除结果中的重复行 ####
```python
#只能得到该列所有的不重复数据
cur.execute("select distinct column_name from table_name;")

#得到不重复数据。先进行分组，之后选择
cur.execute("select *,count(distinct name) from table_name group by column_name")
```

>SELECT COUNT(*) FROM table_name,获取表的行数
[参考资料](http://blog.csdn.net/guocuifang655/article/details/3993612)


## Delete Data ##
在MySQL中有两种方法可以删除数据，一种是DELETE语句，另一种是TRUNCATE语句。DELETE语句可以通过WHERE对要删除的记录进行选择。而使用TRUNCATE将删除表中的所有记录。DELETE语句更灵活。

#### TRUNCATE语句 ####
```python
#一定注意备份，此命令将删除此表的所有数据
cur.execute("TRUNCATE table_name")

cur.close()
conn.commit()
conn.close()		

```

#### DELETE语句 ####
**要经过测试后方可使用**

`delete from 表名 where 删除条件`

```python
#删除条件同查询操作
cur.execute("DELETE FROM table_name WHERE sum > 100")
```

## Update Data ##
更新操作用于更新数据表的的数据，以下实例将 EMPLOYEE 表中的 SEX 字段为 'M' 的 AGE 字段递增 1：

```python
cur.excute("UPDATE EMPLOYEE SET AGE = AGE + 1 WHERE SEX = '%c'" % ('M'))
try:
   # 执行SQL语句
   cursor.execute(sql)
   # 提交到数据库执行
   db.commit()
except:
   # 发生错误时回滚
   db.rollback()

# 关闭数据库连接
db.close()
```


## cursor游标对象属性及方法 ##

#### 1、cursor执行命令的方法 ####

`callproc(sql, procname, args)`   执行存储过程,接收参数为存储过程名和参数列表，返回值为受影响的行数

`execute(sql,param, args)`     执行单条sql语句，接收参数param,返回值为args受影响的行数

`executemany(sql,param, args)`  执行多条sql语句，接收参数param,返回值为args受影响的行数

`next()`            使用迭代对象得到结果的下一行

`close()`           关闭游标

#### 2、cursor用来接收返回值的方法 ####

`fetchone()`        返回一条结果行

`fetchall(self)`       匹配所有剩余结果

`fetchmany(size-cursor,arraysize)`  匹配结果的下几行

`rowcount`         读取数据库表中的*行数*，最后一次execute()返回或影响的行数

`scroll(self, value, mode=’relative’)`     移动指针到某一行。如果mode=’relative’,则表示从当前所在行移动value条，如果mode=’absolute’,则表示从结果集的第一行移动value条

`arraysize`        使用fetchmany()方法时一次取出的记录数，默认为1

`discription`      返回游标的活动状态，包括（7元素）：
(name,type_code,display_size,internal_size,precision,scale,null_ok)其中name, type_code是必须的。

`lastrowid`        返回最后更新行的ID（可选），如果数据库不支持，返回None


## 错误处理 ##

![](http://oyug2kd6x.bkt.clouddn.com//mysql/error.png)
