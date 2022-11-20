---
date: '2009/02/11'
---

<img src='https://img1.baidu.com/it/u=3504749565,1557117143&fm=253&fmt=auto&app=138&f=JPEG?w=382&h=236' />

`sqlldr user/pwd@serviceName control=load.ctl`

load.ctl内容如下：
```sql
load data
infile 'C:\Documents and Settings\Administrator\桌面\数据文件.txt'
append into table tableName //append 或者 replace
fields terminated by ';' //分隔符
(
	EMAIL CHAR,
	GENDER CHAR,
	AGE CHAR
)
```
