---
title: 数据库入门之视图游标存储过程触发器
tags: [MySQL]
---

**数据库视图：**
1.常用于把多表联结的数据保存在一个视图里面，可以重用SQL语句，保护数据等；
2.创建规则：命名唯一，不能有索引，可以和表一起使用，视图本身没有数据；
3.CREATE VIEW来创建，DROP VIEW来删除；
4.例子：把所有厂商的邮箱地址保存在一个视图；
CREATE VIEW emails123 AS
SELECT * FROM vendor WHERE e_mail IS NOT NULL;

SELECT * FROM emails123;
5.更大的用途在多个表联结得到的数据，重新格式化数据，比如给地址值加上横杠括号之类的；
6.更新视图更新的是原数据，有分组等语句不能更新；
SHOW CREATE VIEW emails1;---显示创建视图的语句；
<!-- more -->

**存储过程**
简介：为以后使用而保留的一条或多条MySQL语句；
优缺点：简单，高效；缺点：编写复杂，需要权限等；
***执行存储过程***
1.MySQL执行存储过程的语句为CALL;
2.创建存储过程：
```
CREATE PROCEDURE productpricing()
BEGIN 
	SELECT AVG(pct_price) AS priceaverage
  FROM product;
END;
```
该示例把产品价格显示出来。
productpricing()括号里面可以接受参数；BEGIN...END来限定存储过程体；
3.调用存储过程：
CALL productpricing();
4.删除存储过程：
DROP PROCEDURE productpricing;

5.MySQL的默认语句分隔符为';',使用DELIMTTER可以更改分隔符:
eg:DELIMITER

***使用参数存储数据：***
6.1创建存储过程：
```
CREATE PROCEDURE productpricing(
    OUT pl DECIMAL(8,2),
    OUT ph DECIMAL(8,2),
    OUT pa DECIMAL(8,2)
)
BEGIN
    SELECT Min(pct_price)
    INTO pl 
    FROM product;
    SELECT Max(pct_price)
    INTO ph 
    FROM product;
    SELECT Avg(pct_price)
    INTO pa 
    FROM product;
END
```
6.2 . 调用存储过程：
```
CALL productpricing(@pricehigh,@pricelow,@priceaverage);
```
注：所有MySQL变量都必需以@开始；
6.3 显示由存储过程得到的MySQL的值：
```
SELECT @pricehigh,@pricelow,@priceaverage;
```

**游标**
需要在返回的结果集上移动一行或多行时使用。只能用于存储过程（MySQL）；
使用游标步骤：
在使用游标前，必须声明（定义）它。实际上是声明要使用的SQL语句。
结束游标使用时，要关闭游标。
1.声明游标：
```
CREATE PROCEDURE productorders()
BEGIN 
   DECLARE pctIds CURSOR
   FOR 
   SELECT id FROM product;
END;
```
2.使用游标：
OPEN pctIds;
3.关闭游标：
CLOSE pctIds;
4.游标遍历还有插入数据库：
(以下的数据库运行出问题):
```
CREATE PROCEDURE productorders()
BEGIN 
   DECLARE done BOOLEAN DEFAULT 0;
   DECLARE t VARCHAR;

   DECLARE pctIds CURSOR
   FOR 
   SELECT id FROM product;
   
   DECLARE CONTINUE HANDLER FOR  SQLSTATE '02000' SET done = 1;
   

	 OPEN　pctIds;
   
    REPEAT 
      FETCH pctIds INTO t;
      INSERT INTO c (id) VALUES(t);
    UNTIL done END REPEAT;
   
    CLOSE pctIds;
END;
```
**触发器**
触发器是MySQL响应以下任意语句而自动执行的一条MySQL：DELETE;INSERT;UPDATE;
创建给出四个信息：
1.唯一的触发器名称（在一个表中唯一，最好一个库唯一）；2.触发器关联的表；3.触发器应该响应的活动；4.触发器何时执行；
简单的触发器：
该触发器在插入product表时，
```
CREATE TRIGGER newproduct 
AFTER INSERT 
ON product 
FOR EACH ROW select '没有这个生的信息，不可选课！' into @args; 
```
注：每个表支持6个触发器，DELETE;INSERT;UPDATE之前或之后；单一触发器不能与多个事件或多个表关联。

**事务处理**
事务：一组SQL语句；
回退：撤销指定SQL语句；
提交：将未存储的SQL语句结果写入数据库；
保留点：设置临时占位符，可以对它发布回退；

```
CREATE PROCEDURE productpricing(
    OUT pl INT,
    OUT ph INT,
    OUT pa TIMESTAMP
)
BEGIN
    SELECT COUNT(*)
    INTO pl 
    FROM test WHERE score>=60 GROUP BY date;
   SELECT COUNT(*)
    INTO ph 
    FROM test WHERE score<60 GROUP BY date;
    SELECT DISTINCT date FROM test 
    INTO pa;
END


CALL productpricing(@pricehigh,@pricelow,@priceaverage);

SELECT @pricehigh,@pricelow,@priceaverage;
```