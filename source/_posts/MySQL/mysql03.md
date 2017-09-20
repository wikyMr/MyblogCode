---
title: 数据库入门之插入删除更新
tags: [MySQL]
---

**数据库数据插入：**
***1.插入完整的行***
INSERT INTO product VALUES('5','2','r2',500);--->不建议的写法
缺陷：高度依赖与表中的依赖次序，列的次序变动之后易出错；
改进：
INSERT INTO product(id,pct_name,vend_id,pct_price) VALUES('6','2','r2',500);
可忽略的列：给出默认值或者允许为NULL;
提高整体性能：
INSERT操作可能会比较耗时，优先SELECT语句的做法：
INSERT LOW_PRIORITY INTO product(id,pct_name,vend_id,pct_price) VALUES('6','2','r2',500);

***2.插入多个行***
INSERT LOW_PRIORITY INTO product(id,pct_name,vend_id,pct_price) 
VALUES('6','2','r2',500),('7','2','r2',600);
注：单条INSERT语句处理多个插入，性能会比多个INSERT语句要快！

<!-- more -->

***3.插入检索的数据***
INSERT LOW_PRIORITY INTO product(id,pct_name,vend_id,pct_price) 
SELECT id,`name`,vend_id,price FROM productnew;
可以迅速插入多行；不一定要求列明匹配。

**数据库数据的更新**
不要忽略WHERE字句，否则会更新表的所有行；
UPDATE product 
SET pct_name='桌子'
 WHERE id='1'
或者：
UPDATE product 
SET pct_name='桌子',
    pct_price=600
 WHERE id='1'

**数据库数据的删除**
不要忽略WHERE字句，否则会删除表中的所有行；
DELETE FROM product WHERE id='1';
删除所有的行更快的方法：TRUNCATE TABLE语句；

***删除更新的建议***
1.数据特别重要的，要先对其进行SELECT保证过滤的是正确的结果；
2.除非对全部数据操作，否则要使用WHERE语句。

**数据库的创建和操纵(对表的行列进行操作最好进行备份)**
CREATE TABLE c(
    id int NOT NULL AUTO_INCREMENT,
    name char(100) NOT NULL,
    PRIMARY KEY(id)
) ENGINE=InnoDB

AUTO_INCREMENT:
1.插入时可以覆盖默认值；
2.确认AUTO_INCREMENT值： SELECT LAST_INSERT_ID() FROM c;


***引擎类型***
ENGINE = InnoDB,MEMORY,MyISAM;
InnoDB:支持事物处理，不支持全文搜索;
MyISAM：性能极高，不支持事物处理，支持全文搜索;

***更新表***
增加一行：
ALTER TABLE c 
ADD c_phone CHAR(20);

删除一行：
ALTER TABLE c 
DROP COLUMN c_phone;

***添加主键***
ALTER TABLE product
ADD CONSTRAINT fk_vendor_id
FOREIGN KEY (vend_id) REFERENCES vendor(id);

注：fk_vendor_id为约束名称；
小心ALTER TABLE:最好对数据库进行备份，删除了一列，会把那一列的数据全部丢失。

***删除表***
DROP TABLE c;
