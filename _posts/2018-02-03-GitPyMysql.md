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

mysql的数据格式主要包括5类：[data type of mysql](http://blog.csdn.net/bzhxuexi/article/details/43700435)

```python

conn = MySQLdb.connect("localhost","testuser","test123","TESTDB" )
conn.set_character_set('utf8')
# 使用cursor()方法获取操作游标 
cur = conn.cursor()
#
cur.execute("INSERT INTO table_name (Row_name1, Row_name2 ,Row_name3 ,Row_name4) VALUES (%s, %s, %s, %s)",
                    (value1, value2,value3, value4))

cur.close()
conn.commit()
conn.close()					

```

## select Data ##




>资料来源于：[Python科学计算之Pandas详解，pythonpandas](http://www.bkjia.com/Pythonjc/1189627.html)
>          [十分钟搞定pandas](http://python.jobbole.com/84416/)