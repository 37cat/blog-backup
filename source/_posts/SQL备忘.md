---
title: SQL备忘
date: 2018-05-08 14:12:17
---

最近在复习sql语句的写法，简单做一下记录。

<!--more-->

###### 随机取出10条数据

>SELECT * FROM table_name ORDER BY RANDOM() LIMIT 10;

###### 随机取出10%的数据

>SELECT * FROM table_name ORDER BY RANDOM() LIMIT 0.1*(SELECT COUNT(*) FROM table_name);

###### ORDER BY按列位置排序

>SELECT prod_id, prod_price, prod_name
>FROM Products
>ORDER BY 2, 3;

ORDER BY排序的另一种写法，“2， 3”代表“prod_price, prod_name”,之前没用过的写法，记录一下

###### GROUP BY配合HAVING使用

HAVING的使用类似与WHERE，HAVING过滤针对的是分组

>SELECT vend_id, COUNT(*) AS num_prods
>FROM Products
>WHERE prod_price >= 4    --先选择出price大于等于4的产品
>GROUP BY vend_id  --按供应商分组
>HAVING COUNT(*) >= 2;    --供应商的产品数大于等于2

###### GROUP BY和ORDER BY的区别

虽然有的DBMS会是的GROUP BY子句排序输出，但不能依赖，如果想要得到排序的输出需指定ORDER BY子句

###### JOIN操作

1. 内联结（INNER　JOIN）也叫等值链接

>FROM table1 INNER JOIN table2 ON tabel1.id = table2.id
与
>FROM table1, table2 where tabel1.id = table2.id
等价，都是取table1、table2的笛卡儿积

2. 自联结

>SELECT c1.cust_id, c1.cust_name, c1.cust_contact
>FROM Customers AS c1, Customers AS c2   --同一个表通过使用别名出现两次
>WHERE c1.cust_name = c2.cust_name
>AND c2.cust_contact = 'Jim Jones';

3. 自然联结

自然联结要求你只能选择那些唯一的列，一般通过对一个表使用通配
符（SELECT *），而对其他表的列使用明确的子集来完成。

>SELECT C.*, O.order_num, O.order_date, OI.prod_id, OI.quantity, OI.item_price
>FROM Customers AS C, Orders AS O, OrderItems AS OI
>WHERE C.cust_id = O.cust_id
>AND OI.order_num = O.order_num
>AND prod_id = 'RGAN01';

4. 外联结(OUTER JOIN)

外联结可以联结两表中没有关联行的行，分为左外联结（LEFT）、右外联结（RIGHT）和全外联结（FULL）

###### UNION操作

拼接操作

对比join，UNION把一张表拼在了另一张表的下方，而JOIN则是拼在了右边

UNION会自动消除重复的行，如果需要保留则使用UNION ALL

UNION操作如果要使用排序则将OREDR BY放在最后对所有结果排序，不允许只对UNION中的一部分排序

###### INSERT

1. 一次插入一行数据

>INSERT INTO table(column1, column2, column3)
>VALUES(value1, value2, value3);

可以插入全部列，也可以是部分，没有提供的则为NULL

INTO关键词在有的DBMS中可以省略，但建议保留方便在不同DBMS之间移植

2. INSERT SELECT

>INSERT INTO table(column1, column2, column3)
>SELECT column1, column2, column3
>FROM table2;

3. 复制表

>SELECT *
>INTO new_table
>FROM table;

>CREATE new_table
>AS SELECT *
>FROM table;

###### 视图（VIEW）

作为视图，它不包含任何列或数据，包含的是一个查询

创建

> CREATE VIEW view_name AS SELECT 一些条件

删除

> DROP VIEW view_name

###### 事务（TRANSACTION）

可以回退的语句有：

INSERT、UPDATE、DELETE

不能回退的有：

SELECT（也无必要）、CREATE、DROP

一般语法是：

>BEGIN TRANSACTION
>...
>COMMIT TRANSACTION

###### 游标（CURSOR）

游标（cursor）是一个存储在 DBMS 服务器上的数据库查询，它不是一条SELECT语句，而是被该语句检索出来的结果集。在存储了游标之后，应用程序可以根据需要滚动或浏览其中的数据。

游标主要用于交互式应用，其中用户需要滚动屏幕上的数据，并对数据进行浏览或做出更改。

1. 创建游标：

>DECLARE cursor_name CURSOR
>FOR
>SELECT 一些条件

针对于MySQL

2. 使用游标：

打开游标

>OPEN CURSOR cursor_name

访问游标数据

>FETCH cursor_name INTO

关闭游标

>CLOSE cursor_name

###### 约束

1. 主键

PRIMARY KEY

2. 外键：

外键是表中的一列，其值必须列在另一表的主键中。

在定义外键后， DBMS 不允许删除在另一个表中具有关联行的行。

>CREATE TABLE Orders
>(
>order_num INTEGER NOT NULL PRIMARY KEY,
>order_date DATETIME NOT NULL,
>cust_id CHAR(10) NOT NULL REFERENCES   Customers(cust_id)
>);

3. 唯一性约束

UNIQUE

4. 检查性约束

>CREATE TABLE OrderItems
>(
>order_num INTEGER NOT NULL,
>order_item INTEGER NOT NULL,
>prod_id CHAR(10) NOT NULL,
>quantity INTEGER NOT NULL CHECK (quantity > 0),
>item_price MONEY NOT NULL
>);
---

P.s. 参考的书是《SQL必知必会（第四版）》
