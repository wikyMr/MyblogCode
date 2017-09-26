---
title: 数据库MySQL入门之表的建立及查询及全文搜索
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

<!--more -->
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
***范围检查：BETWEEN AND，大于小于***
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
***NOT操作符：***
用于否定它之后所跟的任何条件；

***数据分组***
GROUP BY语句：
字句中不能是别名，只能是检索列或者表达式。
必须出现在WHERE语句之后，ORDER BY语句之前。

疑问：
SELECT pid,COUNT(*) FROM test
GROUP BY pid WITH ROLLUP
WITH ROLLUP是什么意思？

注：WITH ROLLUP加多一个汇总信息。
| pid         | COUNT(*) | 
| --------    | -----:   | 
| 13          | 3        |
| 14          | 2        |
| null        | 5        |
其实是在最后一行加多一个汇总信息，与GROUP BY的参数有关。
***过滤分组***
WHERE过滤行，HAVING过滤分组；事实上，目前为止，所学的所有类型的WHERE字句都可以用HAVING来替代。

不懂的地方：
test:
| id         | gid  | 
| --------   | -----: | 
| 001        | 13     |  
| 002        | 13     |  
| 003        | 14     | 
| 004        | 14     | 
| 005        | 13     |  
SELECT pid,COUNT(*) FROM test
HAVING COUNT(pid)>0
结果：
| pid         | COUNT(*) | 
| --------    | -----:   | 
| 13          | 5        |

***联结表***：
CROSS JOIN：两个表的笛卡尔积;
INNER JOIN不加ON判断返回的也是笛卡尔积，加ON相当于内联查询。
LEFT JOIN ....ON....
左边的返回所有的列（即使右边的表在ON下无匹配值）
例如：
要列出每个产品的价格，生产厂家信息（对应的vendor_id可能为空），可以这样写：
SELECT pct_name,pct_price,ven_name 
from product pct LEFT JOIN vendor ven ON ven.id=pct.vend_id;
那么即使无对应的生产厂家信息也能得到对应的结果（显示为null）。
我的理解是右边的表可以与左边的表根据ON条件多次匹配，如无匹配结果则返回null（右边的表所有列都会保留！！）；


***组合查询***
1.UNION必须是两条或两条以上的SELECT语句组成。
2.每个查询要包含相同的列，累的数据类型必须兼容。
3.UNION会自动从结果集里面去除重复的行。不去重的话用UNION ALL组合结果。
4.只能出现依稀ORDER BY语句，出现在最后一条SELECT语句之后（会排序所有的结果）;

***全文本搜索***
MySQL数据库引擎分类：常用的有MyISAM和InNoDB，前者支持全文本搜索。
创建数据库是支持指定引擎：
CREAT TABLE a{
  ...
} ENGINE=MyISAM;
***使用全文搜索：***
1.使用FULLTEXT(columName)会对对应的列进行索引；
2.使用Match()和Against()执行全文本搜索；
3.Match(xx)指定xx列，Against('yy')指定搜索的表达式。
4.返回值按等级排序，等级划分：在文本的位置靠前则等级高；（这是比LIKE有优势的地方，而且索引的存在也使得搜索更加的快）；
代替方法：使用LIKE:
eg: LIKE '%yy%';

***布尔文本搜索***
可以指定以什么开头排除以什么包含什么的搜索；
