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

## Insert Data ##

`table_name eg: tianqi.weather`

mysql的数据格式主要包括5类：[data type of mysql](http://blog.csdn.net/bzhxuexi/article/details/43700435)

```python
conn = MySQLdb.connect("localhost","testuser","test123","TESTDB" )
conn.set_character_set('utf8')
# 使用cursor()方法获取操作游标 
cur = conn.cursor()
#
cur.execute("INSERT INTO table_name (column_name1, column_name2 ,column_name3 ,column_name4) VALUES (%s, %s, %s, %s)",
                    (value1, value2,value3, value4))

cur.close()
conn.commit()
conn.close()					
```

## Select Data ##

>[参考资料](http://bbs.csdn.net/topics/390407669),[参考资料](http://blog.csdn.net/changjiangbuxi/article/details/13169861)

`SELECT column_name,column_name FROM table_name [WHERE Clause] [LIMIT N][ OFFSET M]`

>查询语句中FROM后可以使用一个或者多个表，表之间使用逗号(,)分割，并使用WHERE语句来设定查询条件。

>"*"代替其他字段，SELECT语句会返回表的所有字段数据，即满足条件的一行数据。否则只显示所选列数据。

>可以使用 WHERE 语句来包含任何条件，可用and,or连接。

>可以使用 LIMIT 属性来设定返回的记录数。

>可以通过OFFSET指定SELECT语句开始查询的数据偏移量。默认情况下偏移量为0。

#### 选择所有数据 ####
```python
cur.execute("select * from table_name")
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




>资料来源于：[Python科学计算之Pandas详解，pythonpandas](http://www.bkjia.com/Pythonjc/1189627.html)
>          [十分钟搞定pandas](http://python.jobbole.com/84416/)