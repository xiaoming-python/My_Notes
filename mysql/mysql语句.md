# MySQL服务
-  退出mysql交互式
```mysql
exit;
```
- 修改密码
```mysql
ALTER USER 'username'@'host' IDENTIFIED BY 'new_password';  
```
# MySQL数据库操作
- 指定数据库
```mysql
USE 数据库名;
```
- 创建数据库
```mysql
CREATE DATABASE 数据库名; 
```
 - 列出当前 MySQL 服务器上所有的数据库
 ```mysql
 SHOW DATABASES;
 ```
- 直接删除数据库，不检查是否存在
```mysql
DROP DATABASE <database_name>;
```
- 删除数据库，如果存在的话
```mysql
DROP DATABASE [IF EXISTS] <database_name>;
```

# MySQL 数据类型
MySQL 中定义数据字段的类型对数据库的优化是非常重要的。

MySQL 支持多种类型，大致可以分为三类：数值、日期/时间和字符串(字符)类型。
## 数值类型

MySQL 支持所有标准 SQL 数值数据类型。

这些类型包括严格数值数据类型(INTEGER、SMALLINT、DECIMAL 和 NUMERIC)，以及近似数值数据类型(FLOAT、REAL 和 DOUBLE PRECISION)。

关键字INT是INTEGER的同义词，关键字DEC是DECIMAL的同义词。

BIT数据类型保存位字段值，并且支持 MyISAM、MEMORY、InnoDB 和 BDB表。

作为 SQL 标准的扩展，MySQL 也支持整数类型 TINYINT、MEDIUMINT 和 BIGINT。下面的表显示了需要的每个整数类型的存储和范围。

|类型|大小|范围（有符号）|范围（无符号）|用途|
|---|---|---|---|---|
|TINYINT|1 Bytes|(-128，127)|(0，255)|小整数值|
|SMALLINT|2 Bytes|(-32 768，32 767)|(0，65 535)|大整数值|
|MEDIUMINT|3 Bytes|(-8 388 608，8 388 607)|(0，16 777 215)|大整数值|
|INT或INTEGER|4 Bytes|(-2 147 483 648，2 147 483 647)|(0，4 294 967 295)|大整数值|
|BIGINT|8 Bytes|(-9,223,372,036,854,775,808，9 223 372 036 854 775 807)|(0，18 446 744 073 709 551 615)|极大整数值|
|FLOAT|4 Bytes|(-3.402 823 466 E+38，-1.175 494 351 E-38)，0，(1.175 494 351 E-38，3.402 823 466 351 E+38)|0，(1.175 494 351 E-38，3.402 823 466 E+38)|单精度  <br>浮点数值|
|DOUBLE|8 Bytes|(-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308)|0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308)|双精度  <br>浮点数值|
|DECIMAL|对DECIMAL(M,D) ，如果M>D，为M+2否则为D+2|依赖于M和D的值|依赖于M和D的值|小数值|

---

## 日期和时间类型

表示时间值的日期和时间类型为DATETIME、DATE、TIMESTAMP、TIME和YEAR。

每个时间类型有一个有效值范围和一个"零"值，当指定不合法的MySQL不能表示的值时使用"零"值。

TIMESTAMP类型有专有的自动更新特性，将在后面描述。

|类型|大小  <br>( bytes)|范围|格式|用途|
|---|---|---|---|---|
|DATE|3|1000-01-01/9999-12-31|YYYY-MM-DD|日期值|
|TIME|3|'-838:59:59'/'838:59:59'|HH:MM:SS|时间值或持续时间|
|YEAR|1|1901/2155|YYYY|年份值|
|DATETIME|8|'1000-01-01 00:00:00' 到 '9999-12-31 23:59:59'|YYYY-MM-DD hh:mm:ss|混合日期和时间值|
|TIMESTAMP|4|'1970-01-01 00:00:01' UTC 到 '2038-01-19 03:14:07' UTC<br><br>结束时间是第 **2147483647** 秒，北京时间 **2038-1-19 11:14:07**，格林尼治时间 2038年1月19日 凌晨 03:14:07|YYYY-MM-DD hh:mm:ss|混合日期和时间值，时间戳|

---

## 字符串类型

字符串类型指CHAR、VARCHAR、BINARY、VARBINARY、BLOB、TEXT、ENUM和SET。该节描述了这些类型如何工作以及如何在查询中使用这些类型。

|类型|大小|用途|
|---|---|---|
|CHAR|0-255 bytes|定长字符串|
|VARCHAR|0-65535 bytes|变长字符串|
|TINYBLOB|0-255 bytes|不超过 255 个字符的二进制字符串|
|TINYTEXT|0-255 bytes|短文本字符串|
|BLOB|0-65 535 bytes|二进制形式的长文本数据|
|TEXT|0-65 535 bytes|长文本数据|
|MEDIUMBLOB|0-16 777 215 bytes|二进制形式的中等长度文本数据|
|MEDIUMTEXT|0-16 777 215 bytes|中等长度文本数据|
|LONGBLOB|0-4 294 967 295 bytes|二进制形式的极大文本数据|
|LONGTEXT|0-4 294 967 295 bytes|极大文本数据|

**注意**：char(n) 和 varchar(n) 中括号中 n 代表字符的个数，并不代表字节个数，比如 CHAR(30) 就可以存储 30 个字符。

CHAR 和 VARCHAR 类型类似，但它们保存和检索的方式不同。它们的最大长度和是否尾部空格被保留等方面也不同。在存储或检索过程中不进行大小写转换。

BINARY 和 VARBINARY 类似于 CHAR 和 VARCHAR，不同的是它们包含二进制字符串而不要非二进制字符串。也就是说，它们包含字节字符串而不是字符字符串。这说明它们没有字符集，并且排序和比较基于列值字节的数值值。

BLOB 是一个二进制大对象，可以容纳可变数量的数据。有 4 种 BLOB 类型：TINYBLOB、BLOB、MEDIUMBLOB 和 LONGBLOB。它们区别在于可容纳存储范围不同。

有 4 种 TEXT 类型：TINYTEXT、TEXT、MEDIUMTEXT 和 LONGTEXT。对应的这 4 种 BLOB 类型，可存储的最大长度不同，可根据实际情况选择。

---

## 枚举与集合类型（Enumeration and Set Types）

- **ENUM**: 枚举类型，用于存储单一值，可以选择一个预定义的集合。
- **SET**: 集合类型，用于存储多个值，可以选择多个预定义的集合。

---

## 空间数据类型（Spatial Data Types）

GEOMETRY, POINT, LINESTRING, POLYGON, MULTIPOINT, MULTILINESTRING, MULTIPOLYGON, GEOMETRYCOLLECTION: 用于存储空间数据（地理信息、几何图形等）。


# MySQL数据表操作
- 创建数据表，column1,...表中的列名，datatype,...每列的数据类型，int长整数，char(x)x个字符的字符串
```mysql
CREATE TABLE 数据表名 (column1 datatype,column2 datatype,...);  
```
- 列出当前数据库上的数据表
```mysql
SHOW TABLES;
```
- 查看数据表的结构
```mysql
desc 数据表名;
```
- 直接删除表，不检查是否存在
```mysql
DROP TABLE table_name;
```
- 会检查是否存在，如果存在则删除表
```mysql
DROP TABLE [IF EXISTS] table_name;
```
 -  table_name 是你要插入数据的表的名称。column1,... 是表中的列名。value1,... 是要插入的具体数值。
 ```mysql
 INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...),(value1, value2, value3, ...);
 ```
- 读取数据表(选择所有列的所有行)
```mysql
select * from 数据表;
```
- 选择特定列的所有行
```mysql
SELECT username, email FROM users; 
```
- 统一修改：把多行的同一列改成相同的值。table_name 是你要更新数据的表的名称,column1, column2, ... 是你要更新的列的名称。value1, value2,
 ... 是新的值，用于替换旧的值。WHERE condition 是一个可选的子句，用于指定更新的行。如果省略 WHERE 子句，将更新表中的所有行
 ```mysql
 UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
 ```
 - 批量修改为不同值：CASE WHEN 结构
   如果要把多个用户的 `username` 改成不同的新名字，一条 SQL 就能搞定：
```mysql
UPDATE users
SET username = CASE id
    WHEN 1 THEN '张三_new'
    WHEN 2 THEN '李四_new'
    WHEN 3 THEN '王五_new'
    ELSE username  -- 不处理的保持原值
END
WHERE id IN (1, 2, 3);
```
`CASE` 相当于“根据不同 id 赋予不同值”，非常快捷。
-    table_name 是你要删除数据的表的名称。WHERE condition 是一个可选的子句，用于指定删除的行。
如果省略 WHERE 子句，将删除表中的所有行
```mysql
DELETE FROM table_name
WHERE condition;
```
# MySQL WHERE 子句
WHERE 子句类似于程序语言中的 if 条件，根据 MySQL 表中的字段值来读取指定的数据
你可以使用 AND 或者 OR 指定一个或多个条件
WHERE 子句也可以运用于 SQL 的 DELETE 或者 UPDATE 命令
例如：
- 等于条件(WHERE username = 'test'查询username='test' 有返回结果，没有返回false)
```mysql
SELECT * FROM users WHERE username = 'test';
```
- 不等于条件
```mysql
SELECT * FROM users WHERE username != 'runoob'; 
```
 - 大于条件
 ```mysql
 SELECT * FROM products WHERE price > 50.00; 
 ```
- 小于条件
```mysql
SELECT * FROM orders WHERE order_date < '2023-01-01';
```
-  大于等于条件
```mysql
SELECT * FROM employees WHERE salary >= 50000;
```
- 小于等于条件
```mysql
SELECT * FROM students WHERE age <= 21;
```
- 组合条件（AND、OR）
 ```mysql
 SELECT * FROM products WHERE category = 'Electronics' AND price > 100.00;
 ```
 - 模糊匹配条件（LIKE）
 ```mysql
 SELECT * FROM customers WHERE first_name LIKE 'J%'; 
 ```
- IN 条件
```mysql
SELECT * FROM countries WHERE country_code IN ('US', 'CA', 'MX');
```
- NOT 条件
```mysql
SELECT * FROM products WHERE NOT category = 'Clothing'; 
```
- BETWEEN 条件
```mysql
SELECT * FROM orders WHERE order_date BETWEEN '2023-01-01' AND '2023-12-31'; 
```
- IS NULL 条件
```mysql
SELECT * FROM employees WHERE department IS NULL; 
```
- IS NOT NULL 条件
```mysql
SELECT * FROM customers WHERE email IS NOT NULL; 
```
# MySQL LIKE 子句
SELECT column1, column2, ...FROM table_name WHERE column_name LIKE pattern;  --
百分号通配符 %,% 通配符表示零个或多个字符
_ 通配符表示一个字符
列如
- 以下 SQL 语句将选择产品名称的第二个字符为 'a' 的所有产品。
```mysql
SELECT * FROM products WHERE product_name LIKE '_a%';
```
# MySQL UNION 操作符
- 类似与并集，会去重（UNION ALL 不去重）
```mysql
SELECT city FROM customers UNION SELECT city FROM suppliers ORDER BY city;
```
# MySQL ORDER BY(排序) 语句
- MySQL ORDER BY(排序) 语句可以按照一个或多个列的值进行升序（ASC）或降序（DESC）排序
```mysql
SELECT column1, column2, ... FROM table_name ORDER BY column1 [ASC | DESC], column2 [ASC | DESC], ...;

```
## MySQL GROUP BY 语句
- - `column1`：指定分组的列。
- `aggregate_function(column2)`：对分组后的每个组执行的聚合函数。例如sum()求和
- `table_name`：要查询的表名。
- `condition`：可选，用于筛选结果的条件。
```mysql
SELECT column1, aggregate_function(column2)
FROM table_name
WHERE condition
GROUP BY column1;
```
实例数据：
```cmd
mysql> select * from orders;
+------+------+--------+
| id   | name | amount |
+------+------+--------+
|    1 | tang |    100 |
|    2 | liu  |    200 |
|    3 | jake |    200 |
|    4 | jake |    300 |
+------+------+--------+
4 rows in set (0.005 sec)
```
## sum()求和函数
- 返回选中之和

例如：
```mysql
select name ,sum(amount) as ok from orders group by name;
```
输出：
```print
+------+------+
| name | ok   |
+------+------+
| tang |  100 |
| liu  |  200 |
| jake |  500 |
+------+------+
3 rows in set (0.023 sec)
```
## count()聚合函数
- 返回选中总共多少行
例如：
```mysql
select name,count(*) from orders group by name;
```
输出：
```print
+------+----------+
| name | count(*) |
+------+----------+
| tang |        1 |
| liu  |        1 |
| jake |        2 |
+------+----------+
3 rows in set (0.015 sec)
```
## WITH ROLLUP
- WITH ROLLUP 可以实现在分组统计数据基础上再进行相同的统计（SUM,AVG,COUNT…）。
例如：
```mysql
select name,count(*) from orders group by name with rollup;
```
输出：
```print
+------+----------+
| name | count(*) |
+------+----------+
| jake |        2 |
| liu  |        1 |
| tang |        1 |
| NULL |        4 |
+------+----------+
4 rows in set (0.041 sec)
```
### coalesce
- 我们可以使用 coalesce 来设置一个可以取代 NUll 的名称，coalesce 语法：
- 参数说明：如果 a等于null，则选择 b；如果 b等于null,则选择 c；如果 a不等于null,则选择 a；如果 a b c 都为 null ，则返回为 null（没意义）。

```mysql
SELECT coalesce(name, '总数'), SUM(signin) as signin_count FROM  employee_tbl GROUP BY name WITH ROLLUP;
```
输出：
```print
+-----------------------+----------+
| coalesce(name,"zong") | count(*) |
+-----------------------+----------+
| jake                  |        2 |
| liu                   |        1 |
| tang                  |        1 |
| zong                  |        4 |
+-----------------------+----------+
4 rows in set (0.007 sec)
```
# MySQL数据表连接
你可以在 SELECT, UPDATE 和 DELETE 语句中使用 MySQL 的 JOIN 来联合多表查询。

JOIN 按照功能大致分为如下三类：
## INNER JOIN（内连接,或等值连接）
获取两个表中字段匹配关系的记录。
INNER JOIN可以简写成JOIN
- INNER JOIN 返回两个表中满足连接条件的匹配行，以下是 INNER JOIN 语句的基本语法：
```mysql
SELECT column1, column2, ...
FROM table1
INNER JOIN table2 ON table1.column_name = table2.column_name;
```
- `column1`, `column2`, ... 是你要选择的列的名称，如果使用 `*` 表示选择所有列。
- `table1`, `table2` 是要连接的两个表的名称。
- `table1.column_name = table2.column_name` 是连接条件，指定了两个表中用于匹配的列。将连接满足条件的行
实例数据：
```cmd
mysql> select * from data;
+------+----------+-----------------------+
| id   | name     | input                 |
+------+----------+-----------------------+
|    1 | xiaotang | my name is xiaotang   |
|    2 | 小唐     | 我的名字叫小唐        |
|    3 | 小明     | 你说你有点难追        |
|    4 | oi       | 哦，原来是这样        |
|    5 | hh       | 有点意思              |
|    6 | 爱       | love                  |
|    7 | 布       | daws                  |
|    8 | me       | 我回来了              |
+------+----------+-----------------------+
8 rows in set (0.003 sec)
mysql> select * from orders;
+------+------+--------+
| id   | name | amount |
+------+------+--------+
|    1 | tang |    100 |
|    2 | liu  |    200 |
|    3 | jake |    200 |
|    4 | jake |    300 |
+------+------+--------+
4 rows in set (0.005 sec)
```
实例：
```cmd
mysql> select orders.amount,orders.name,data.input,data.name from orders inner join data on orders.id=data.id;
+--------+------+-----------------------+----------+
| amount | name | input                 | name     |
+--------+------+-----------------------+----------+
|    100 | tang | my name is xiaotang   | xiaotang |
|    200 | liu  | 我的名字叫小唐        | 小唐     |
|    200 | jake | 你说你有点难追        | 小明     |
|    300 | jake | 哦，原来是这样        | oi       |
+--------+------+-----------------------+----------+
4 rows in set (0.009 sec)
```
- inner join 可以省略inner使用join，效果一样
- 表名可以在后面写别名，例如：`data a`   a为别名
- `select orders.amount,data.input from orders join data on orders.id=data.id;`
  等价与
  `select a.amount,b.input from orders a join data b on a.id=b.id;`
## LEFT JOIN（左连接）
- LEFT JOIN 是一种常用的连接类型，尤其在需要返回左表中所有行的情况下。
当右表中没有匹配的行时，相关列将显示为 NULL。
在使用 LEFT JOIN 时，请确保理解连接条件并根据需求过滤结果。
MySQL left join 与 join 有所不同，LEFT JOIN 会读取左边数据表的全部数据，即使右边表无对应数据
实例：
```cmd
mysql> select a.name,b.name from data a left join orders b on a.id=b.id;
+----------+------+
| name     | name |
+----------+------+
| xiaotang | tang |
| 小唐     | liu  |
| 小明     | jake |
| oi       | jake |
| hh       | NULL |
| 爱       | NULL |
| 布       | NULL |
| me       | NULL |
+----------+------+
8 rows in set (0.007 sec)
```
## RIGHT JOIN（右连接）
- MySQL RIGHT JOIN 会读取右边数据表的全部数据，即使左边边表无对应数据。
在实际开发过程中，RIGHT JOIN 并不经常使用，因为它可以用 LEFT JOIN 和表的顺序交换来实现相同的效果。
实例：
```cmd
mysql> select a.name,b.name from data a right join orders b on a.id=b.id;
+----------+------+
| name     | name |
+----------+------+
| xiaotang | tang |
| 小唐     | liu  |
| 小明     | jake |
| oi       | jake |
+----------+------+
4 rows in set (0.006 sec)
```
# MySQL NULL 值处理

我们已经知道 MySQL 使用 SELECT 命令及 WHERE 子句来读取数据表中的数据，但是当提供的查询条件字段为 NULL 时，该命令可能就无法正常工作。

在 MySQL 中，NULL 用于表示缺失的或未知的数据，处理 NULL 值需要特别小心，因为在数据库中它可能会导致不同于预期的结果。

为了处理这种情况，MySQL提供了三大运算符:

- **IS NULL:** 当列的值是 NULL,此运算符返回 true。
- **IS NOT NULL:** 当列的值不为 NULL, 运算符返回 true。
- **<=>:** 比较操作符（不同于 = 运算符），当比较的的两个值相等或者都为 NULL 时返回 true。

关于 NULL 的条件比较运算是比较特殊的。你不能使用 = NULL 或 != NULL 在列中查找 NULL 值 。

在 MySQL 中，NULL 值与任何其它值的比较（即使是 NULL）永远返回 NULL，即 NULL = NULL 返回 NULL 。

MySQL 中处理 NULL 使用 IS NULL 和 IS NOT NULL 运算符。
### MySQL 中处理 NULL 值的常见注意事项和技巧

1. 检查是否为 NULL：

要检查某列是否为 NULL，可以使用 IS NULL 或 IS NOT NULL 条件。
```mysql
SELECT * FROM employees WHERE department_id IS NULL;
SELECT * FROM employees WHERE department_id IS NOT NULL;
```

2. 使用 COALESCE 函数处理 NULL：

COALESCE 函数可以用于替换为 NULL 的值，它接受多个参数，返回参数列表中的第一个非 NULL 值：
```mysql
SELECT product_name, COALESCE(stock_quantity, 0) AS actual_quantity
FROM products;
```

以上 SQL 语句中，如果 stock_quantity 列为 NULL，则 COALESCE 将返回 0。

3. 使用 IFNULL 函数处理 NULL：

IFNULL 函数是 COALESCE 的 MySQL 特定版本，它接受两个参数，如果第一个参数为 NULL，则返回第二个参数。

```mysql
SELECT product_name, IFNULL(stock_quantity, 0) AS actual_quantity
FROM products;
```
4. NULL 排序：

在使用 ORDER BY 子句进行排序时，NULL 值默认会被放在排序的最后。如果希望将 NULL 值放在最前面，可以使用 ORDER BY column_name ASC NULLS FIRST，反之使用 ORDER BY column_name DESC NULLS LAST。

```mysql
SELECT product_name, price
FROM products
ORDER BY price ASC NULLS FIRST;
```
5. 使用 <=> 操作符进行 NULL 比较：

<=> 操作符是 MySQL 中用于比较两个表达式是否相等的特殊操作符，对于 NULL 值的比较也会返回 TRUE。它可以用于处理 NULL 值的等值比较。

```mysql
SELECT * FROM employees WHERE commission <=> NULL;
```

6. 注意聚合函数对 NULL 的处理：

在使用聚合函数（如 COUNT, SUM, AVG）时，它们会忽略 NULL 值，因此可能会得到不同于预期的结果。如果希望将 NULL 视为 0，可以使用 COALESCE 或 IFNULL。

```mysql
SELECT AVG(COALESCE(salary, 0)) AS avg_salary FROM employees;
```

这样即使 salary 为 NULL，聚合函数也会将其视为 0。

# MySQL 正则表达式
MySQL 可以通过 **LIKE ...%** 来进行模糊匹配。

MySQL 同样也支持其他正则表达式的匹配， MySQL 中使用 REGEXP 和 RLIKE操作符来进行正则表达式匹配。


下表中的正则模式可应用于 REGEXP 操作符中。

| 模式         | 描述                                                                             |
| ---------- | ------------------------------------------------------------------------------ |
| ^          | 匹配输入字符串的开始位置。如果设置了 RegExp 对象的 Multiline 属性，^ 也匹配 '\n' 或 '\r' 之后的位置。            |
| $          | 匹配输入字符串的结束位置。如果设置了RegExp 对象的 Multiline 属性，$ 也匹配 '\n' 或 '\r' 之前的位置。             |
| .          | 匹配除 "\n" 之外的任何单个字符。要匹配包括 '\n' 在内的任何字符，请使用像 '[.\n]' 的模式。                        |
| [...]      | 字符集合。匹配所包含的任意一个字符。例如， '[abc]' 可以匹配 "plain" 中的 'a'。                             |
| [^...]     | 负值字符集合。匹配未包含的任意字符。例如， '[^abc]' 可以匹配 "plain" 中的'p'。                             |
| p1\|p2\|p3 | 匹配 p1 或 p2 或 p3。例如，'z\|food' 能匹配 "z" 或 "food"。'(z\|f)ood' 则匹配 "zood" 或 "food"。 |
| *          | 匹配前面的子表达式零次或多次。例如，zo* 能匹配 "z" 以及 "zoo"。* 等价于{0,}。                              |
| +          | 匹配前面的子表达式一次或多次。例如，'zo+' 能匹配 "zo" 以及 "zoo"，但不能匹配 "z"。+ 等价于 {1,}。                |
| {n}        | n 是一个非负整数。匹配确定的 n 次。例如，'o{2}' 不能匹配 "Bob" 中的 'o'，但是能匹配 "food" 中的两个 o。           |
| {n,m}      | m 和 n 均为非负整数，其中n <= m。最少匹配 n 次且最多匹配 m 次。                                       |
## 正则表达式匹配的字符类
- `.`：匹配任意单个字符。
- `^`：匹配字符串的开始。
- `$`：匹配字符串的结束。
- `*`：匹配零个或多个前面的元素。
- `+`：匹配一个或多个前面的元素。
- `?`：匹配零个或一个前面的元素。
- `[abc]`：匹配字符集中的任意一个字符。
- `[^abc]`：匹配除了字符集中的任意一个字符以外的字符。
- `[a-z]`：匹配范围内的任意一个小写字母。
- `[0-9]`：匹配一个数字字符。
- `\w`：匹配一个字母数字字符（包括下划线）。
- `\s`：匹配一个空白字符。
## 使用 REGEXP 进行模式匹配
REGEXP 是用于进行正则表达式匹配的运算符。
REGEXP 用于检查一个字符串是否匹配指定的正则表达式模式，以下是 REGEXP 运算符的基本语法：

```mysql
SELECT column1, column2, ...
FROM table_name
WHERE column_name REGEXP 'pattern';
```

**参数说明：**

- `column1`, `column2`, ... 是你要选择的列的名称，如果使用 `*` 表示选择所有列。
- `table_name` 是你要从中查询数据的表的名称。
- `column_name` 是你要进行正则表达式匹配的列的名称。
- `'pattern'` 是一个正则表达式模式。
## 使用 RLIKE 进行模式匹配

RLIKE 是 MySQL 中用于进行正则表达式匹配的运算符，与 REGEXP 是一样的，RLIKE 和 REGEXP 可以互换使用，没有区别。

```mysql
SELECT column1, column2, ...
FROM table_name
WHERE column_name RLIKE 'pattern';
```

# MySQL 事务
1、用 BEGIN, ROLLBACK, COMMIT 来实现

**BEGIN 或 START TRANSACTION**：开用于开始一个事务。
**ROLLBACK** 事务回滚，取消之前的更改。
**COMMIT**：事务确认，提交事务，使更改永久生效。
- BEGIN 或 START TRANSACTION -- 用于开始一个事务或者使用 START TRANSACTION

```mysql
BEGIN; 
```

- COMMIT -- 用于提交事务，将所有的修改永久保存到数据库：

```mysql
COMMIT;
```

- ROLLBACK -- 用于回滚事务，撤销自上次提交以来所做的所有更改：

```mysql
ROLLBACK;
```

- SAVEPOINT -- 用于在事务中设置保存点，以便稍后能够回滚到该点：

```mysql
SAVEPOINT savepoint_name;
```
- ROLLBACK TO SAVEPOINT -- 用于回滚到之前设置的保存点：

```mysql
ROLLBACK TO SAVEPOINT savepoint_name;
```
# MySQL ALTER 命令

当我们需要修改数据表名或者修改数据表字段时，就需要使用到 MySQL ALTER 命令。

MySQL 的 ALTER 命令用于修改数据库、表和索引等对象的结构。

ALTER 命令允许你添加、修改或删除数据库对象，并且可以用于更改表的列定义、添加约束、创建和删除索引等操作。

ALTER 命令非常强大，可以在数据库结构发生变化时进行灵活的修改和调整。
## 1. 添加列
```mysql
ALTER TABLE table_name
ADD COLUMN new_column_name datatype;
```
实例:
以下 SQL 语句在 employees 表中添加了一个名为 birth_date 的日期列。
```mysql
ALTER TABLE employees  
ADD COLUMN birth_date DATE;
```
## 2. 修改列的数据类型
```mysql
ALTER TABLE TABLE_NAME  
MODIFY COLUMN column_name new_datatype;
```
实例：
以下 SQL 语句将 employees 表中的 salary 列的数据类型修改为 DECIMAL(10,2)。
```mysql
ALTER TABLE employees  
MODIFY COLUMN salary DECIMAL(10,2);
```
## 3. 修改列名

```mysql
ALTER TABLE table_name
CHANGE COLUMN old_column_name new_column_name datatype;
```
实例：
以下 SQL 语句将 employees 表中的某个列的名字由 old_column_name 修改为 new_column_name，并且可以同时修改数据类型。
```mysql
ALTER TABLE employees  
CHANGE COLUMN old_column_name new_column_name VARCHAR(255);  
```
## 4. 删除列

```mysql
ALTER TABLE table_name
DROP COLUMN column_name;
```
实例：
以下 SQL 语句将 employees 表中的 birth_date 列删除。
```mysql
ALTER TABLE employees  
DROP COLUMN birth_date;  
```
## 5. 添加 PRIMARY KEY

```mysql
ALTER TABLE table_name
ADD PRIMARY KEY (column_name);
```
实例：
以下 SQL 语句在 employees 表中添加了一个主键。

```mysql
ALTER TABLE employees  
ADD PRIMARY KEY (employee_id);  
```

## 6.删除 PRIMARY KEY

```mysql
ALTER TABLE testalter_tbl DROP PRIMARY KEY;
```
## 7. 添加 FOREIGN KEY

```mysql
ALTER TABLE child_table
ADD CONSTRAINT fk_name
FOREIGN KEY (column_name)
REFERENCES parent_table (column_name);
```
实例：
以下 SQL 语句在 orders 表中添加了一个外键，关联到 customers 表的 customer_id 列。

```mysql
ALTER TABLE orders  
ADD CONSTRAINT fk_customer  
FOREIGN KEY (customer_id)  
REFERENCES customers (customer_id);  
```

## 8. 修改表名

```mysql
ALTER TABLE old_table_name
RENAME TO new_table_name;
```
实例：
以下 SQL 语句将表名由 employees 修改为 staff。

```mysql
ALTER TABLE employees  
RENAME TO staff;
```
# MySQL 索引
MySQL 索引是一种数据结构，用于加快数据库查询的速度和性能。
拿汉语字典的目录页（索引）打比方，我们可以按拼音、笔画、偏旁部首等排序的目录（索引）快速查找到需要的字。
## 普通索引
### 创建索引
使用 **CREATE INDEX** 语句可以创建普通索引。

普通索引是最常见的索引类型，用于加速对表中数据的查询。

**CREATE INDEX** 的语法：

```mysql
CREATE INDEX index_name
ON table_name (column1 [ASC|DESC], column2 [ASC|DESC], ...);
```
- `CREATE INDEX`: 用于创建普通索引的关键字。
- `index_name`: 指定要创建的索引的名称。索引名称在表中必须是唯一的。
- `table_name`: 指定要在哪个表上创建索引。
- `(column1, column2, ...)`: 指定要索引的表列名。你可以指定一个或多个列作为索引的组合。这些列的数据类型通常是数值、文本或日期。
- `ASC`和`DESC`（可选）: 用于指定索引的排序顺序。默认情况下，索引以升序（ASC）排序。

以下实例假设我们有一个名为 students 的表，包含 id、name 和 age 列，我们将在 name 列上创建一个普通索引。

```mysql
CREATE INDEX idx_name ON students (name);
```

上述语句将在 students 表的 name 列上创建一个名为 idx_name 的普通索引，这将有助于提高通过姓名进行搜索的查询性能。

需要注意的是，如果表中的数据量较大，索引的创建可能会花费一些时间，但一旦创建完成，查询性能将会显著提高。
### 修改表结构(添加索引)

我们可以使用 **ALTER TABLE** 命令可以在已有的表中创建索引。

ALTER TABLE 允许你修改表的结构，包括添加、修改或删除索引。

ALTER TABLE 创建索引的语法：

```mysql
ALTER TABLE table_name
ADD INDEX index_name (column1 [ASC|DESC], column2 [ASC|DESC], ...);
```

- `ALTER TABLE`: 用于修改表结构的关键字。
- `table_name`: 指定要修改的表的名称。
- `ADD INDEX`: 添加索引的子句。`ADD INDEX`用于创建普通索引。
- `index_name`: 指定要创建的索引的名称。索引名称在表中必须是唯一的。
- `(column1, column2, ...)`: 指定要索引的表列名。你可以指定一个或多个列作为索引的组合。这些列的数据类型通常是数值、文本或日期。
- `ASC`和`DESC`（可选）: 用于指定索引的排序顺序。默认情况下，索引以升序（ASC）排序。

下面是一个实例，我们将在已存在的名为 employees 的表上创建一个普通索引:

```mysql
ALTER TABLE employees
ADD INDEX idx_age (age);
```

上述语句将在 employees 表的 age 列上创建一个名为 idx_age 的普通索引。
### 创建表的时候直接指定

我们可以在创建表的时候，你可以在 **CREATE TABLE** 语句中直接指定索引，以创建表和索引的组合。

```mysql
CREATE TABLE table_name (
  column1 data_type,
  column2 data_type,
  ...,
  INDEX index_name (column1 [ASC|DESC], column2 [ASC|DESC], ...)
);
```
- `CREATE TABLE`: 用于创建新表的关键字。
- `table_name`: 指定要创建的表的名称。
- `(column1 data_type, column2 data_type, ...)`: 定义表的列名和数据类型。你可以指定一个或多个列作为索引的组合。这些列的数据类型通常是数值、文本或日期。
- `INDEX`: 用于创建普通索引的关键字。
- `index_name`: 指定要创建的索引的名称。索引名称在表中必须是唯一的。
- `(column1, column2, ...)`: 指定要索引的表列名。你可以指定一个或多个列作为索引的组合。这些列的数据类型通常是数值、文本或日期。
- `ASC`和`DESC`（可选）: 用于指定索引的排序顺序。默认情况下，索引以升序（ASC）排序。

下面是一个实例，我们要创建一个名为 students 的表，并在 age 列上创建一个普通索引。

```mysql
CREATE TABLE students (
  id INT PRIMARY KEY,
  name VARCHAR(50),
  age INT,
  INDEX idx_age (age)
);
```

在上述实例中，我们在 students 表的 age 列上创建了一个名为 idx_age 的普通索引。

### 删除索引的语法

我们可以使用 **DROP INDEX** 语句来删除索引。

DROP INDEX 的语法：

```mysql
DROP INDEX index_name ON table_name;
```

- `DROP INDEX`: 用于删除索引的关键字。
- `index_name`: 指定要删除的索引的名称。
- `ON table_name`: 指定要在哪个表上删除索引。

使用 ALTER TABLE 语句删除索引的语法如下：

ALTER TABLE table_name
DROP INDEX index_name;

- `ALTER TABLE`: 用于修改表结构的关键字。
- `table_name`: 指定要修改的表的名称。
- `DROP INDEX`: 用于删除索引的子句。
- `index_name`: 指定要删除的索引的名称。

以下实例假设我们有一个名为 employees 的表，并在 age 列上有一个名为 idx_age 的索引，现在我们要删除这个索引：

DROP INDEX idx_age ON employees;

或使用 **ALTER TABLE** 语句：

```mysql
ALTER TABLE employees
DROP INDEX idx_age;
```

这两个命令都会从 employees 表中删除名为 idx_age 的索引。

如果该索引不存在，执行命令时会产生错误。因此，在删除索引之前最好确认该索引是否存在，或者使
## 唯一索引

在 MySQL 中，你可以使用 CREATE UNIQUE INDEX 语句来创建唯一索引。

唯一索引确保索引中的值是唯一的，不允许有重复值。

### 创建索引

```mysql
CREATE UNIQUE INDEX index_name
ON table_name (column1 [ASC|DESC], column2 [ASC|DESC], ...);
```
- `CREATE UNIQUE INDEX`: 用于创建唯一索引的关键字组合。
- `index_name`: 指定要创建的唯一索引的名称。索引名称在表中必须是唯一的。
- `table_name`: 指定要在哪个表上创建唯一索引。
- `(column1, column2, ...)`: 指定要索引的表列名。你可以指定一个或多个列作为索引的组合。这些列的数据类型通常是数值、文本或日期。
- `ASC`和`DESC`（可选）: 用于指定索引的排序顺序。默认情况下，索引以升序（ASC）排序。

以下是一个创建唯一索引的实例： 假设我们有一个名为 employees的 表，包含 id 和 email 列，现在我们想在email列上创建一个唯一索引，以确保每个员工的电子邮件地址都是唯一的。

```mysql
CREATE UNIQUE INDEX idx_email ON employees (email);
```

以上实例将在 employees 表的 email 列上创建一个名为 idx_email 的唯一索引。

### 修改表结构添加索引

我们可以使用 **ALTER TABLE** 命令来创建唯一索引。

**ALTER TABLE**命令允许你修改已经存在的表结构，包括添加新的索引。

```mysql
ALTER table table_name 
ADD CONSTRAINT unique_constraint_name UNIQUE (column1, column2, ...);
```

- `ALTER TABLE`: 用于修改表结构的关键字。
- `table_name`: 指定要修改的表的名称。
- `ADD CONSTRAINT`: 这是用于添加约束（包括唯一索引）的关键字。
- `unique_constraint_name` : 指定要创建的唯一索引的名称，约束名称在表中必须是唯一的。
- `UNIQUE (column1, column2, ...)`: 指定要索引的表列名。你可以指定一个或多个列作为索引的组合。这些列的数据类型通常是数值、文本或日期。

以下是一个使用 **ALTER TABLE** 命令创建唯一索引的实例：假设我们有一个名为 employees 的表，包含 id 和 email 列，现在我们想在 email 列上创建一个唯一索引，以确保每个员工的电子邮件地址都是唯一的。

```mysql
ALTER TABLE employees
ADD CONSTRAINT idx_email UNIQUE (email);
```

以上实例将在 employees 表的 email 列上创建一个名为 idx_email 的唯一索引。

请注意，如果表中已经有重复的 email 值，那么添加唯一索引将会失败。在创建唯一索引之前，你可能需要确保表中的 email 列没有重复的值。

### 创建表的时候直接指定

我们也可以在创建表的同时，你可以在 **CREATE TABLE** 语句中使用 **UNIQUE** 关键字来创建唯一索引。

这将在表创建时同时定义唯一索引约束。

**CREATE TABLE** 语句中创建唯一索引的语法：

```mysql
CREATE TABLE table_name (
  column1 data_type,
  column2 data_type,
  ...,
  CONSTRAINT index_name UNIQUE (column1 [ASC|DESC], column2 [ASC|DESC], ...)
);
```

- `CREATE TABLE`: 用于创建新表的关键字。
- `table_name`: 指定要创建的表的名称。
- `(column1 data_type, column2 data_type, ...)`: 定义表的列名和数据类型。你可以指定一个或多个列作为索引的组合。这些列的数据类型通常是数值、文本或日期。
- `CONSTRAINT`: 用于添加约束的关键字。
- `index_name`: 指定要创建的唯一索引的名称。约束名称在表中必须是唯一的。
- `UNIQUE (column1, column2, ...)`: 指定要索引的表列名。
- `ASC`和`DESC`（可选）: 用于指定索引的排序顺序。默认情况下，索引以升序（ASC）排序。

以下是一个在创建表时创建唯一索引的实例：假设我们要创建一个名为 employees 的表，其中包含 id、name 和 email 列，我们希望 email 列的值是唯一的，因此我们要在创建表时定义唯一索引。

```mysql
CREATE TABLE employees (
  id INT PRIMARY KEY,
  name VARCHAR(50),
  email VARCHAR(100) UNIQUE
);
```

在这个例子中，email 列被定义为唯一索引，因为在它的后面加上了 UNIQUE 关键字。

请注意，使用 **UNIQUE** 关键字后，索引名称将自动生成，你也可以根据需要指定索引名称。

---

### 使用ALTER 命令添加和删除索引

有四种方式来添加数据表的索引：

- **ALTER TABLE tbl_name ADD PRIMARY KEY (column_list):** 该语句添加一个主键，主键列中的值必须唯一，主键的列的列表，可以是一个或多个列，不能包含 NULL 值。 。
    
- **ALTER TABLE tbl_name ADD UNIQUE index_name (column_list):** 这条语句创建索引的值必须是唯一的（除了NULL外，NULL可能会出现多次）。
- **ALTER TABLE tbl_name ADD INDEX index_name (column_list):** 添加普通索引，索引值可出现多次。
- **ALTER TABLE tbl_name ADD FULLTEXT index_name (column_list):**该语句指定了索引为 FULLTEXT ，用于全文索引。

以下实例为在表中添加索引。

mysql> ALTER TABLE testalter_tbl ADD INDEX (c);

你还可以在 ALTER 命令中使用 DROP 子句来删除索引。尝试以下实例删除索引:

mysql> ALTER TABLE testalter_tbl DROP INDEX c;用错误处理机制来处理可能的错误情况。

---
## 显示索引信息

你可以使用 SHOW INDEX 命令来列出表中的相关的索引信息。

可以通过添加 \G 来格式化输出信息。

SHOW INDEX 语句：:

mysql> SHOW INDEX FROM table_name\G
........

- `SHOW INDEX`: 用于显示索引信息的关键字。
- `FROM table_name`: 指定要查看索引信息的表的名称。
- `\G`: 格式化输出信息。

执行上述命令后，将会显示指定表中所有索引的详细信息，包括索引名称（Key_name）、索引列（Column_name）、是否是唯一索引（Non_unique）、排序方式（Collation）、索引的基数（Cardinality）等。
# MySQL 临时表

MySQL 临时表在我们需要保存一些临时数据时是非常有用的。

临时表只在当前连接可见，当关闭连接时，MySQL 会自动删除表并释放所有空间。

在 MySQL 中，临时表是一种在当前会话中存在的表，它在会话结束时会自动被销毁。

MySQL 临时表只在当前连接可见，如果你使用PHP脚本来创建 MySQL 临时表，那每当 PHP 脚本执行完成后，该临时表也会自动销毁。

如果你使用了其他 MySQL 客户端程序连接 MySQL 数据库服务器来创建临时表，那么只有在关闭客户端程序时才会销毁临时表，当然你也可以手动销毁。

### 创建临时表

```mysql
CREATE TEMPORARY TABLE temp_table_name (
  column1 datatype,
  column2 datatype,
  ...
);
```
或者简写为：

```mysql
CREATE TEMPORARY TABLE temp_table_name AS
SELECT column1, column2, ...
FROM source_table
WHERE condition;
```
### 插入数据到临时表

```mysql
INSERT INTO temp_table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```

### 查询临时表

```mysql
SELECT * FROM temp_table_name;
```

### 修改临时表

临时表的修改操作与普通表类似，可以使用 ALTER TABLE 命令。

```mysql
ALTER TABLE temp_table_name
ADD COLUMN new_column datatype;
```

### 删除临时表

临时表在会话结束时会自动被销毁，但你也可以使用 DROP TABLE 明确删除它。

```mysql
DROP TEMPORARY TABLE IF EXISTS temp_table_name;
```

