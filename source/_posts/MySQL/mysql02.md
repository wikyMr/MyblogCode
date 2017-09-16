---
title: 数据库MySQL入门
tags: [MySQL]
---
**数据表相关：**
主键：唯一的标识某一行，可以由多列来组成，不能为空；
1.SHOW DATABSES;----------输出可用数据库的列表；
2.USE TEST(数据库的名字);------使用某个数据库；
3.SHOW TABLES;  ----显示数据库所有的表格；

**SELECT相关：**
4.SELECT prod_name FROM products;----从表中检索某一列；
5.去掉结果中的某一些行：
SELECT DISTINCT ven_id  FROM products;
注：一些书写的规范：
**SQL关键字使用大写，对所有的列名和表名使用小写；**
6.限制结果：
SELECT prod_name FROM products LIMIT 5； 返回不多于5行的结果；
SELECT prod_name FROM products LIMIT 5，5； 返回第6行开始不多于5行的结果；
7.完全限定的表名：
SELECT pro_name FROM products;
也可以这么写(完全限定的表名)：
SELECT pro_name FROM crashcource.products;
***8.排序：ORDER BY 语句：***
SELECT pro_name FROM products ORDER BY pro_name;
按多列排序：
SELECT pro_name FROM products ORDER BY pro_name，pro_price;
按照声明的顺序先后进行排序；
逆序排序：
SELECT pro_name FROM products ORDER BY pro_name DESC,pro_price DESC;
9.ORDER BY加LIMIT组合可以找出一个最大或者最小值：
SELECT prod_price FROM products ORDER BY prod_price DESC LIMIT 1;
<!-- more -->
***9.范围检查：BETWEEN AND，大于小于***
SELECT pro_name,pro_price
FROM products
WHERE pro_price
BETWEEN 100 AND 160;
NULL值检查：//测试不通过
***10.AND OR 连接筛选语句***
SELECT pro_name,pro_price
FROM products 
WHERE pro_id=1 OR pro_id=2 AND pro_price>50;
返回的结果不是预期的结果，
pro_name pro_price
Aa 50
Bb 100
因为AND的优先级大于OR，要使用花括号分组操作符；
SELECT pro_name,pro_price
FROM products 
WHERE (pro_id=1 OR pro_id=2) AND pro_price>50;
***11.IN 操作符***
用于指定条件范围：(上面的语句也可以写成这样的形式)
SELECT pro_name,pro_price
FROM products 
WHERE pro_id IN (1,2) AND pro_price>50;
注：IN操作符比OR操作符清单执行更快；
***NOT***操作符：
用于否定它之后所跟的任何条件；
