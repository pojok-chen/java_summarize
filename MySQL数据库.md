# MySQL数据库

## 数据库概述

**什么是数据库(Database，DB)？**

​	数据库是将大量数据保存起来，通过计算机的加工处理后可以实现高效访问的数据集合。

**什么是数据库管理系统（manager system，MS）？**

​	用来管理数据库的操作系统就是数据库管理系统。

**为什么要使用DBMS？**

​	我们知道，在以前，我们完全可以使用EXCEL这样的表格来保存管理数据，那为什么我们还需要数据库管理系统呢？

- 可以保证共享实现
- 可以操作大量的数据，远超Excel可操作的数据。
- 可以更加自动化的存储读取数据。
- 采用数据库管理系统来对数据进行管理，更加的安全。

​    **我们通常所说的MySQL数据库，其实它是一种数据库管理系统和“数据存储仓库”的结合，它包含着数据库管理系统和真正存储数据的数据库。**

​	**只有将数据仓库和管理系统结合起来，才可以称之为DBMS。而RDBMS代表关系型数据库。**

**什么是关系型数据库？**

​	所谓的“关系”是指什么？数据库对信息的管理是以表的形式来管理的，而关系型数据库通常可以将不同表中的信息链接起来，已达到更复杂的多个表之间信息的联动效果。

### 数据库的结构

​	在现在的MySQL数据库中，其结构通常是一种客户端/服务器的数据库结构，如图所示：

![数据库结构](../图片/数据库截图/数据库结构.png)

### 数据库中的表结构

​	关系数据库通过类似 Excel 工作表那样的、由行和列组成的二维表来管理数据。用来管理数据的二维表在关系数据库中简称为**表**。	

![数据库中的表](../图片/数据库截图/数据库中的表.png)

**根据 SQL 语句的内容返回的数据同样必须是二维表的形式 。**

 **关系数据库必须以行为单位进行数据读写** 。

## 查询语句基础

### select语句查询基础

#### 列的查询

​	select语句的查询语法如下：

```sql
SELECT < 列名 > ，…… FROM < 表名 > 
-- SELECT 子句中列举了希望从表中查询出的列的名称
-- 而 FROM 子句则指定了选取出数据的表的名称。
-- from字句在从表中查询时是必须的，在进行运算时可以不需要from子句
```

​	该 SELECT 语句包含了 SELECT 和 FROM 两个子句（clause） 。子句是 SQL 语句的组成要素，是以 SELECT 或者 FROM 等作为起始的短语。	

**查询结果中列的顺序和SELECT 子句中的顺序相同，行的顺序就不一定和表中的一样了**

#### 查询出表中的所有列

​	通过select语句查询出所有的列的语法如下：

```SQL
SELECT 　 * FROM < 表名 >
-- 其中*代表所有的列
```

​	![查询语句中的空行会报错](../图片/数据库截图/查询语句中的空行会报错.png)

#### 为列设定别名

​	SQL 语句可以使用 AS 关键字为列设定别名。例如：

```SQL
SELECT product_id AS id,
	   product_name AS name,
       sale_price AS price
FROM Product;
```

其执行结果为：

![带别名的查询语句](../图片/数据库截图/带别名的查询语句.png)

其结果如图所示，查询出来的列名就是查询语句中定义的别名。

​	别名还可以使用中文，不过必须是双引号括起来，如下面的例子：

```SQL
SELECT product_id AS " 商品编号 ",
	   product_name AS " 商品名称 ",
       purchase_price AS " 进货单价 "
FROM Product;
```

#### 常数的查询

​	SELECT 子句中不仅可以书写列名，还可以书写常数。例如：

```SQL
SELECT 
		' 商品 ' AS string, 
		38 AS number, 
		'2009-02-24' AS date,
		product_id, 
		product_name
FROM Product;
```

![查询常数](../图片/数据库截图/查询常数.png)

其查询结果如上图所示。

**在SQL语句中使用字符串或者日期常数时，必须使用单引号( ' )将其括起来。**

#### 删除重复的行

​	想要删除重复行时，可以通过在 SELECT 子句中使用DISTINCT关键字来实现

```SQL
SELECT DISTINCT product_type
FROM Product;
```

​	执行前在product_type这一列中有很多的重复项，如图：

![重复的行](../图片/数据库截图/重复的行.png)

​	执行上面的去重操作后，如图所示：

![去除重复的行](../图片/数据库截图/去除重复的行.png)

**在使用 DISTINCT 时， NULL 也被视为一类数据。 NULL 存在于多行中时 ， 也会被合并为一条 NULL 数据。**

​	下面考虑对多列进行去重：

```SQL
SELECT DISTINCT product_type, regist_date
FROM Product
```

在对多列进行去重操作时，只有两列的数据在某两行都相同时才会去重，并不是认为的可以分别对某一列进行去重。

​	**DISTINCT 关键字只能用在第一个列名之前。**

#### 根据where语句来选择记录

​	SELECT 语句通过 WHERE 子句来指定查询数据的条件。

​	在 SELECT 语句中使用 WHERE 子句的语法如下所示：

```SQL
SELECT < 列名 >, ……
FROM < 表名 >
WHERE < 条件表达式 >;
```

```SQL
SELECT product_name, product_type
FROM Product
WHERE product_type = ' 衣服 ';
```

![where语句](../图片/数据库截图/where语句.png)

WHERE 子句中的“ product _ type = ' 衣服 ' ”就是用来表示查询条件的表达式（条件表达式）。等号是比较两边的内容是否相等的符号，上述条件就是将 product _ type 列的值和 ' 衣服 ' 进行比较，判断是否相等。 Product 表的所有记录都会被进行比较。

​	其执行结果是这样的：**首先通过WHERE 子句查询出符合指定条件的记录，然后再选取出 SELECT 语句指定的列**

​	当然，也可以不选取作为查询条件的列：

```SQL
SELECT product_name
FROM Product
WHERE product_type = ' 衣服 '
```

**SQL 中子句的书写顺序是固定的，不能随意更改。 WHERE 子句必须紧跟在 FROM 子句之后**

#### 注释的书写

​	MySQL中的注释有两种：

- 单行注释：--+空格+注释内容
- 多行注释：/*      */   其之间的就是注释内容

```SQL
-- 单行注释（要注意符号和文字之间的空格不能丢）

/*
	这里写多行注释
*/
```

### 算数运算符和比较运算符

#### 算数运算符

​	例如：

```SQL
-- 将sale_price这一列的数据乘以2倍之后显示出来
SELECT product_name, sale_price,
sale_price * 2 AS "sale_price_x2"
FROM Product
```

其结果，如图所示：

![算数运算符](../图片/数据库截图/算数运算符.png)

SQL语句中支持的四则运算符有加减乘除，如果‘+’号前后位数字或者列中数据是数字的话就会进行加法运算。当然，也可以使用括号来提高运算的优先级。

#### 需要注意null值

​	考虑四则运算中含有null值的问题，例如：

![含有null值的四则运算](../图片/数据库截图/含有null值的四则运算.png)

**其结果全部为null，任何与null进行的运算都为null值。**

#### 比较运算符

​	在上面where语句中使用的‘=’号就是比较运算符，用来比较该符号两边的值是否相等，SQL语句中支持的比较运算符包括：

![比较运算符](../图片/数据库截图/比较运算符.png)

这些比较运算符可以对字符、数字和日期等几乎所有数据类型的列和值进行比较。

下面举一个比较日期的例子：

```SQL
-- 比较日期用的是<,>,<=,>=这些符号，符号两边放日期类型
SELECT product_name, product_type, regist_date
FROM Product
WHERE regist_date < '2009-09-27';
```

其结果为：

![比较日期](../图片/数据库截图/比较日期.png)

**字符串类型的数据原则上按照字典顺序进行排序，不能与数字的大小顺序混淆。**

#### 不能对null值使用不等运算符

​	对某一列中的数据有null存在时，考虑采用is Null或者is   Not  Null运算符，而不应该使用比较运算符，因为比较运算符不适用于null值。例如：

```SQL
-- 查询purchase_price不等于2800的记录
SELECT product_name, purchase_price
FROM Product
WHERE purchase_price <> 2800;
```

查询之前：

![包含null值的记录](../图片/数据库截图/包含null值的记录.png)

可以看出数据中有两条null记录，而执行上面的查询结果后：

![对null值用比较运算符](../图片/数据库截图/对null值用比较运算符.png)

你的实际意图可能只是剔除purchase_price = 2800的记录，但是，它把null值记录也剔除了。

​	所以，对null值使用比较运算时要注意了，SQL提供了对null值的判断操作

```SQL
is Null -- 判断某个值是否为null
is Not Null -- 判断某个值是否不为null
```

```SQL
-- 选取purchase_price等于null时的记录，is not null的用法与其类似
SELECT product_name, purchase_price
FROM Product
WHERE purchase_price IS NULL;
```

### 逻辑运算符

#### Not运算符

​	Not运算符的作用类似于<>（不等号），但是其范围是要比不等号广的。

​	Not不能单独使用，必须要和其它运算符组合起来使用。

​	Not运算符就是用来否定某一条件的，例如：

```SQL
-- 通过Not运算符来否定sale_price >= 1000的情况
SELECT product_name, product_type, sale_price
FROM Product
WHERE NOT sale_price >= 1000;
```

其查询结果相当于sale_price < 1000时的情况：

![not运算符](../图片/数据库截图/not运算符.png)

在这个例子中，使用not运算符是不如直接使用比较运算符来的更直观，但是在某些情况下使用Not运算符更好，要视具体情况而定。

#### AND运算符和OR运算符

​	AND运算符就相当于java中的&&与运算符；

​	OR运算符就相当于java中的或运算符；

​	在 WHERE 子句中使用 AND 运算符或者 OR 运算符，可以对多个查询条件进行组合。	

```SQL
-- 查询所有的单价大于3000的厨房用具
SELECT product_name, purchase_price
FROM Product
WHERE product_type = ' 厨房用具 '
AND sale_price >= 3000;
```

```SQL
-- 查询所有的厨房用具或者是单价大于3000的商品
SELECT product_name, purchase_price
FROM Product
WHERE product_type = ' 厨房用具 '
OR sale_price >= 3000;
```

#### 通过括号强化处理

​	假如，有这样一个例子，让我们来看一下加括号和不加括号的区别：

​	需求是这样的：

​		“商品是办公用品”并且“登记日期是 2009 年 9 月11日或者 2009 年 9 月 20 日”	

不使用括号时的查询语句是这样的：

```SQL
SELECT product_name, product_type, regist_date
FROM Product
WHERE product_type = ' 办公用品 '
AND regist_date = '2009-09-11'
OR regist_date = '2009-09-20'
```

其结果是这样的：

![不使用括号的多个逻辑查询](../图片/数据库截图/不使用括号的多个逻辑查询.png)

看其结果好像有点不对，明明想要办公用品，怎么把厨房用具什么的也查出来了。

而使用括号时的查询语句是这样的：

```SQL
SELECT product_name,product_type,regist_date
FROM product 
WHERE 
product_type = '办公用品'
	AND 
(regist_date = '2009-9-11' OR regist_date = '2009-9-20')
```

其查询结果是：

![使用多个括号后的逻辑查询](../图片/数据库截图/使用多个括号后的逻辑查询.png)

好像这样更符合我们的需求。

**所以，使用括号，就像算数运算符一样，可以类似于提高逻辑运算的优先级，使得加括号的逻辑运算可以先算，所以我们要合理的加括号来使得SQL语句可以更符合我们的预期。**

#### 含有null值的逻辑运算其结果是什么

​	在SQL语句中，‘null>45’这样的逻辑判断其结果是什么？我们知道，null值是不能用比较运算符判断的，必须使用is Null或者is Not Null来进行判断，那么这个判断结果肯定不为真，那么它的结果为假吗？也就是会返回false吗？**不是的，在SQL语句中，对于这一类含null的逻辑运算符判断，其结果为UNKNOWN，也就是不确定的，所以该判断条件的结果不为真也不为假**。

### 对WHERE子句进行组合

#### AND和OR操作符

​	这两个操作符是逻辑运算符，前面已经讲过，就不再赘述了；这两个操作符和where子句组合时就是用来组合一系列的“并且”、“或者”等where筛选条件。**需要注意的是，括号在多个and或者or之间的运用。**

#### BETWEEN...AND...操作符

​	BETWEEN...AND...操作符，从其字面意思上来看，很直观，就是用来指定一个选择范围的。其语法格式为：

```MySQL
SELECT < 列名 1>, < 列名 2>, < 列名 3>, ……
FROM < 表名 >
WHERE 列名（字段） BETWEEN <最小值> AND <最大值>
```

```MySQL
-- 从商品表中查询商品id在0002到0006之间的所有商品
SELECT * FROM product
WHERE product_id BETWEEN 0002 AND 0006
-- 上面的SQL语句就相当于
SELECT * FROM product 
WHERE 
	product_id <= 0006 
AND product_id >= 0002
```

它们的查询结果都为：

![between and 操作符](../图片/数据库截图/between and 操作符.png)

​	好吧，这个操作符是为了帮我们简化的。这应该是MySQL对上面那个大于等于、小于等于和and那一系列操作的封装（自己猜测）。所以适用于它们的规则就适用于BETWEEN...AND...操作符。

#### IN操作符

​	IN操作符用来指定条件范围，范围中的每个条件都可以进行匹配。其格式为：

```mySQL
SELECT < 列名 1>, < 列名 2>, < 列名 3>, ……
FROM < 表名 >
WHERE 列名（字段） IN （取值1，取值2，...）
```

```MySQL
-- 从product表中查询product_id=0001，或者product_id=0005，或者product_id=0008的商品
SELECT * FROM product
WHERE product_id IN (0001,0005,0008)
```

正如上面所描述的，IN操作符相当于OR操作符，是使用OR操作符只能让我们一个个的写，例如上面的IN操作符改成使用OR操作符的话，得要这样写：

```MySQL
SELECT * FROM product
WHERE 
	product_id=0001 
 OR product_id=0005 
 OR product_id=0008
```

这两个查询语句的结果都为：

![IN操作符](../图片/数据库截图/IN操作符.png)

​	使用IN操作符，帮我们简化了OR操作符的使用。同时，使用**IN操作符是要快于OR操作符的，而且在IN操作中可以包含其它的select子句，这是使用IN操作符的优点**。

#### NOT操作符

​	NOT操作符在前面已经说过了，它主要的用途就是用来对逻辑条件进行取反操作的。

​	**同样，在这里它也适用于IN操作符和BETWEEN...AND...操作符。**

#### LIKE操作符

​	前面对where子句中条件的编写中，其条件值都是确定的，有时候我们需要不是那么确定的值来编写条件，例如模糊查询的操作，这里就需要用到LIKE操作符。其基本语法为：

```SQL
SELECT < 列名 1>, < 列名 2>, < 列名 3>, ……
FROM < 表名 >
WHERE 列名（字段） LIKE <搜索字符串> -- 搜索字符串中包含着字面值和通配符
```

上面提到了通配符，在MySQL中，通配符有两种，分别是：

##### 百分号（%）通配符

​	**百分号（%）统配符，在搜索字符串中，用百分号可以表示任意字符出现任意次数。**，例如：

```MySQL
-- 匹配商品类型中含有‘用’字的商品种类
select * from product
where product_type like '%用%' -- 该搜索字符串的意义是，只要字符串中包含‘用’字就可以，‘用’前后甚至可以没有字符，因为%代表任意个字符，当然也可以是0个。
```

其输出结果为：

![like中使用%通配符](../图片/数据库截图/like中使用%通配符.png)

​	**要注意的是，like于%通配符无法组合出代表null值的数据，所以，最后以该字段搜索时，一定不会搜索出该字段上为null的行记录**

##### 下划线（_）通配符

​	**下划线（_）通配符，在搜索字符串中，只能表示单个字符，注意是单个。用该统配符可以用来明确限定该搜索字符串总共有几个字符串，做到相对精确搜索。**

```SQL
SELECT * FROM product
WHERE product_type LIKE '__用具' -- 这里通配符用了两个下划线，代表两个字符
```

输出结果为：

![like语句中下划线通配符的使用](../图片/数据库截图/like语句中下划线通配符的使用.png)

##### 统配符使用注意点

- 不要过度使用通配符，其它操作能达到相同目的的，因优先使用其它操作，因为使用通配符操作会很慢。
- 在必须需要统配符的时候，尽量不要把统配符放在搜索字符串的首字符位置处，因为这样会导致大量的遍历操作，导致搜索很慢。
- 第三点就是要注意通配符的数量及顺讯，尤其是下划线通配符，如果多一个下划线通配符，可能就搜索不到了。

## 聚合与排序

### 对表进行聚合查询

#### 聚合函数

​	通过 SQL 对数据进行某种操作或计算时需要使用函数。例如，计算表中全部数据的行数时，可以使用 COUNT 函数。

​	在SQL语句中的聚合函数有五种：

- COUNT ： 计算表中的记录数（行数）
- SUM ：      计算表中数值列中数据的合计值
- AVG ：       计算表中数值列中数据的平均值
- MAX ：      求出表中任意列中数据的最大值
- MIN ：      求出表中任意列中数据的最小值	

​       通过上面的解释可以看出，SQL中用于汇总的函数被称为聚合函数。所谓聚合，就是将多行函数汇总为一行。

首先我们来看一下product表中的所有数据：

![product表中的所有数据](../图片/数据库截图/product表中的所有数据.png)

接下来我们分别使用一下这五个聚合函数。

##### COUNT聚合函数

​	COUNT函数的作用就是用来计算表中的行数，例如：

```SQL
-- 计算product表中的所有行数
SELECT COUNT(*)
FROM Product;
-- 在count（）括号中填的是一个参数，这里填的是一个*号，代表所有列
```

其输出结果就是一个行数，如下：

![count函数的运用](../图片/数据库截图/count函数的运用.png)

如图所示，只输出了一个行数为8行。

​	而如果使用count函数来单单只计算purchase_price这一列的行数的话会发生什么？

```SQL
-- 计算purchase_price这一列的行数
SELECT COUNT(purchase_price)
from Product
```

其结果为：

![count计算某一列的行数](../图片/数据库截图/count计算某一列的行数.png)

如上图所示，其结果为6行，为什么不是8行？**这是因为，在count函数中，会忽略数据为null的记录。**

​	**同时还要注意的是，只有count函数才可以将*号当为参数传递，其它的聚合函数是不可以的，会报错。**

##### SUM聚合函数

​	使用SUM函数可以计算合计值。例如，计算某一列的总和：

```SQL
-- 计算sale_price这一列的总和
SELECT SUM(sale_price)
FROM Product;
```

其结果为：

![sum函数的运用](../图片/数据库截图/sum函数的运用.png)

如果是分别计算两列的和呢？

```SQL
-- 分别计算sale_price列和purchase_price列的和
SELECT SUM(sale_price), SUM(purchase_price)
FROM Product;
```

其结果为：

![分别计算多列的和](../图片/数据库截图/分别计算多列的和.png)

要注意的是，在purchase_price这一列中是有null值存在的，sum函数肯定是在对某一列的所有值进行相加来得出总数的，那么，不是说还有null值的四则运算的结果必定为null吗？为什么purchase_price这一列还能计算出结果？这是因为：**所有的聚合函数，如果以列名为参数，那么在计算之前就已经把NULL 排除在外了。因此，无论有多少个 NULL 都会被无视。只有COUNT(*)是不忽视null值的。**

​	所以在purchase_price这一列根本就没有去和null值相加，所以计算结果也不为null了。

##### AVG聚合函数

​	AVG聚合函数用来计算平均值。下面看一下其使用：

```SQL
-- 计算sale_price这一列的平均值
SELECT AVG(sale_price)
FROM Product;
```

其结果为：

![AVG函数](../图片/数据库截图/AVG函数.png)

如果计算的这一列包含null值，那结果会不会为null呢？肯定不会，因为上面说了，聚合函数的参数如果是列名的话，那么该聚合函数就会在计算之前先把该列中的null值排除出去然后在进行四则运算。

##### MIN和MAX聚合函数

​	MIN函数用来计算一列的最小值；

​	MAX函数用来计算一列的最大值；

```SQL
-- 分别计算sale_price列中的最大值，purchase_price列中的最小值
SELECT MAX(sale_price), MIN(purchase_price)
FROM Product;
```

其结果为：

![min和max函数](../图片/数据库截图/min和max函数.png)

要注意的是： **MAX/MIN 函数和 SUM/AVG 函数有一点不同，那就是 SUM/AVG 函数只能对数值类型的列使用，也只有对数值类型的列使用才有意义，而 MAX/MIN 函数原则上可以适用于任何数据类型的列。**

​	例如，假如将MIN和MAX函数用在日期类型和字符串类型的列上：

```SQL
-- 计算regist_date列的最小值，product_name列的最大值
SELECT MIN(regist_date),MAX(product_name)
FROM product
-- 其中字符串类型的最大最小可能为根据字典顺序排序的
```

其结果为：

![min和max用在日期和字符串类型上](../图片/数据库截图/min和max用在日期和字符串类型上.png)

##### 使用聚合函数删除重复值

​	拿商品种类来说，表中总共有 3 种商品共 8 行数据，其中衣服2 行，办公用品 2 行，厨房用具 4 行。如果想要计算出商品种类的个数，怎么做比较好呢？

​	我们可以想到，去除重复行数再计算其行数就可以了，那么该怎么实现呢？

```SQL
-- 计算product_type列去重后的记录数
SELECT COUNT(DISTINCT product_type)
FROM Product;
```

其结果为：

![去重后使用聚合函数](../图片/数据库截图/去重后使用聚合函数.png)

请注意，这时 DISTINCT 必须写在括号中。这是因为必须要在计算行数之前删除 product _ type 列中的重复数据。

​	不限于count函数可以去重后使用，其它聚合函数也可以和distinct关键字联合使用，例如SUM函数，

```SQL
-- 计算一下sale_price列去重和不去重分别的合计值
SELECT SUM(sale_price), SUM(DISTINCT sale_price)
FROM Product;
```

其结果为：

![sum去重](../图片/数据库截图/sum去重.png)

结果是不一样的，这是因为在这一列中有两个商品单价都是500元，去重后就会剔除一个500元，只保留一个500元进行计算。

### 对表数据进行分组

​	单纯的使用聚合函数都是对表中的所有的数据进行统计，而使用GROUP BY结合聚合函数的方式就可以对表中数据进行分组后再统计。

#### GROUP BY子句

​	其语法格式为：

```SQL
SELECT < 列名 1>, < 列名 2>, < 列名 3>, ……
FROM < 表名 >
GROUP BY < 列名 1>, < 列名 2>, < 列名 3>, ……
```

看一下其具体的用法：

```SQL
-- 统计每一个商品种类下分别有多少商品
SELECT product_type, COUNT(*)
FROM Product
GROUP BY product_type
```

其结果为：

![根据商品种类统计](../图片/数据库截图/根据商品种类统计.png)

如上所示，未使用 GROUP BY 子句时，结果只有 1 行，而这次的结果却是多行。这是因为不使用 GROUP BY 子句时，是将表中的所有数据作为一组来对待的。而使用 GROUP BY 子句时，会将表中的数据分为多个组进行处理。

​	在 GROUP  BY 子句中指定的列称为聚合键或者分组列。	它也可以像SELECT子句那样通过逗号分隔多个列。

​	**GROUP BY 子句的书写位置也有严格要求，一定要写在FROM 语句之后（如果有 WHERE 子句的话需要写在 WHERE 子句之后）。**

#### 分组列中存在null数据的情况

​	在商品表中，我们知道purchase_price这一列中是存在null值的，那当对这一列进行分组并统计时的结果是怎样的呢？null值会被怎么处理呢？

```SQL
-- 对purchase_price这一列进行分组并统计
SELECT purchase_price, COUNT(*)
FROM Product
GROUP BY purchase_price;
```

其结果为：

![分组统计中包含null数据时的情况](../图片/数据库截图/分组统计中包含null数据时的情况.png)

从结果中看出，**当分组列中包含null时，会将所有的null分为一组数据。**

#### 使用where子句和GROUP BY子句

​	当where子句和GRUOP BY子句都存在时，其语法格式如下：

```SQL
SELECT < 列名 1>, < 列名 2>, < 列名 3>, ……
FROM < 表名 >
WHERE
GROUP BY < 列名 1>, < 列名 2>, < 列名 3>, …… 
```

**像这样使用 WHERE 子句进行汇总处理时，会先根据 WHERE 子句指定的条件进行过滤，然后再进行汇总处理。**

```SQL
-- 从商品类型为衣服的商品中根据purchase_price进行分组，然后统计行数
-- 统计每种折扣价格的衣服数量
SELECT purchase_price, COUNT(*)
FROM Product
WHERE product_type = ' 衣服 '
GROUP BY purchase_price;
```

其结果为：

![where和group by联合使用](../图片/数据库截图/where和group by联合使用.png)

上面的查询过程是这样的，先从表中查出类型为衣服的商品，然后对类型为衣服的商品进行分类，最后再对分类后的每一组数据进行统计记录数。

​	从这里可以看出，聚合函数的执行过程是比较靠后的，一般是在先把数据从表中查出来，然后才会进行统计。

#### 与聚合函数和 GROUP BY 子句有关的常见错误

##### 常见错误一：在 SELECT 子句中书写了多余的列

​	这个错误是说，在使用GROUP BY 子句是，对于select子句中列出的元素有严格的规定，实际上，使用聚合函数时， SELECT 子句中只能存在以下三种元素。

- 常数
- 聚合函数
-  GROUP BY 子句中指定的列名（也就是聚合键）

例如：

```SQL
SELECT product_name, purchase_price, COUNT(*)
FROM Product
GROUP BY purchase_price;
```

这样的语句就会报错，是不允许的，因为在select子句中存在了product_name列名而没有再GROUP BY子句中指定。

​	**要注意的是，这种错误在MySQL中是不会出现的，可以正常执行。也就是说，这种写法是MySQL特有的，它不属于SQL规范语句，所以在MySQL中它可以正常执行，而在其他数据库系统中是会报错的。**

​	**所以，不管是出于哪种考虑，我们都尽量遵循：使用 GROUP BY 子句时， SELECT 子句中不要出现聚合键之外的列名。**

##### 常见错误二：在 GROUP BY 子句中写了列的别名

​	一般在查询的时候，我们可能在select子句中会写多个列，为了使用方便，我们可能会为这些列名定义一个别名，**而如果把这个别名用在group by子句后就会报错**。例如：

```SQL
SELECT product_type AS pt, COUNT(*)
FROM Product
GROUP BY pt;
-- group by子句中使用了前面定义的别名pt
```

当然，这个在MySQL中又不会报错，但是它却不符合SQL语句规范，导致该SQL语句不能通用。

**所以，尽量不要在GROUP BY子句中使用别名。**

##### 常见错误三： GROUP BY 子句的结果能排序吗

​	 **GROUP BY 子句结果的显示是无序的，要想其有序，则需要手动对其排序，也就是使用order by子句。**

##### 常见错误四：在 WHERE 子句中使用聚合函数

​	例如：

```SQL
SELECT product_type, COUNT(*)
FROM Product
WHERE COUNT(*) = 2  -- 在where子句中使用了聚合函数，这是错误的写法
GROUP BY product_type;
```

​	**注意，只有 SELECT 子句和 HAVING 子句（以及之后将要学到的ORDER BY 子句）中能够使用 COUNT 等聚合函数**。

#### 各子句的执行顺序

​	到目前为止，见过了select子句，from子句，where子句以及group by子句，那么它们的执行顺序是怎么样的呢？是不是按照写的顺序来执行的呢？

​	**其执行顺序并不是按照写的顺序来的，其执行顺序是：**

**from子句-->where子句-->group by子句-->select子句**

#### 关于DISTINCT 和 GROUP BY

​	在上面的学习中，看到distinct和group by对于得到商品种类这一处理上，其结果是一样的，例如：

```SQL
select distinct product_type
from product
```

```SQL
select product_type 
from product
group by product_type
```

它们得到的结果都是：

![distinct和group by](../图片/数据库截图/distinct和group by.png)

那么这两个语句分别在什么时候才需要呢？

​	在选择上，对这两个语句可能会存在疑惑，因为它们返回的结果是一样的，而且执行速度也是一样的。

​	而在选择的时候，我们应该遵循这样的规则：**在“想要删除选择结果中的重复记录”时
使用 DISTINCT ，在“想要计算汇总结果”时使用 GROUP BY 。**

### 为聚合结果指定条件

​	使用 COUNT 函数等对表中数据进行汇总操作时，为其指定条件的不是WHERE 子句，而是 HAVING 子句。	

​	WHERE 子句用来指定数据行的条件， HAVING 子句用来指定分组的条件。

#### HAVING子句

​	我们可以使用group by子句来对表中数据进行分组，可以使用where子句和group by子句联合使用的方式来对表中的数据进行限定条件后的分组操作。

​	那么，怎么在对表中数据分组之后再根据条件去筛选呢？这里就要用到HAVING子句了。其语法格式如下：

```SQL
SELECT < 列名 1>, < 列名 2>, < 列名 3>, ……
FROM < 表名 >
GROUP BY < 列名 1>, < 列名 2>, < 列名 3>, ……
HAVING < 分组结果对应的条件 >
```

​	**HAVING 子句必须写在 GROUP BY 子句之后，其在 DBMS 内部的执行顺序也排在 GROUP BY 子句之后。**

```SQL
-- 对商品表根据商品类型进行分组，从分组后的结果中取出包含2个行的某几类商品
SELECT product_type, COUNT(*)
FROM Product
GROUP BY product_type
HAVING COUNT(*) = 2;
```

其结果为：

![having子句](../图片/数据库截图/having子句.png)

从上面的执行结果中看出，本来商品可以分为三类，还有一类是厨房用具，但是因为厨房用具在表中有四个商品，所以在使用HAVING COUNT(*) = 2这一筛选条件后，以为这我们只需要包含2个商品的那种商品种类，所以厨房用具这一类就被在根据类型分组之后被丢弃了，是剩下这两个包含2件商品的种类。

​	再来考虑如下这个例子：

```SQL
-- 这是查看每种分类下商品的平均单价
SELECT product_type,AVG(sale_price)
FROM product 
GROUP BY product_type
```

其结果为：

![每种商品的平均单价](../图片/数据库截图/每种商品的平均单价.png)

假如我想在此基础上再取出平均单价大于等于2500元的商品种类的话应该这样写：

```SQL
-- 取得某种类型的商品的平均单价大于等于2500元的商品种类
SELECT product_type, AVG(sale_price)
FROM Product
GROUP BY product_type
HAVING AVG(sale_price) >= 2500;
```

其结果为：

![某种商品的平均单价的having查询](../图片/数据库截图/某种商品的平均单价的having查询.png)

#### HAVING子句的构成要素

​	HAVING子句和包含GROUP BY子句的SELECT子句的要求一样，对其能出现在HAVING子句中的元素也是有严格限制的：

- 常数
- 聚合函数
- GROUP BY 子句中指定的列名（聚合键）

**在思考 HAVING 子句的使用方法时，把一次汇总后的结果作为 HAVING 子句起始点的话更容易理解。**

#### HAVING子句条件和WHERE子句条件对比

​	有时候看，将条件写在HAVING子句和写在WHERE子句中，其返回结果是一样的，例如，

采用HAVING子句：

```SQL
-- 先对商品表根据类型进行分类，然后找出分类中是衣服的商品种类
SELECT product_type, COUNT(*)
FROM Product
GROUP BY product_type
HAVING product_type = ' 衣服 '
```

采用WHERE子句:

```SQL
-- 先对找出商品中是衣服的种类，然后对其分组，那肯定只有衣服一种类别了
SELECT product_type, COUNT(*)
FROM Product
WHERE product_type = ' 衣服 '
GROUP BY product_type;
```

两种SQL语句的执行结果都是：

![HAVING条件和WHERE条件](../图片/数据库截图/HAVING条件和WHERE条件.png)

结果是一样的，但是其执行逻辑确是不一样的。那我们什么时候该用WHERE子句写条件，什么时候该用HAVING子句写条件呢？大致有这么两个标准：

- 对于需要通过聚合键或者分组列来进行条件限定的话，那么最好将条件写在WHERE子句中。

  这样做的原因是：

  ​	因为WHERE子句是对数据表中的行进行操作，而HAVING子句是对数据表中的分组情况进行操作。

  ​	**能写在WHERE子句中的条件就不要写在HAVING子句中，因为写在WHERE子句中的条件执行速度是要快与HAVING子句中的。**这是因为在使用COUNT等聚合函数操作的时候，数据库内部会进行一些排序等额外的操作，这些操作就会造成数据库处理产生额外的负担，而WHERE子句在排序之前就进行了过滤，所以速度更快。并且，可以对 WHERE 子句指定条件所对应的列创建索引，这样也可以大幅提高处理速度。

### 对查询结果进行排序

#### ORDER BY子句

​	ORDER BY子句用来对查询出得结果排序后再展示，其语法格式如下：

```SQL
SELECT < 列名 1>, < 列名 2>, < 列名 3>, ……
FROM < 表名 >
ORDER BY < 排序基准列 1>, < 排序基准列 2>, ……
```

下面看一个例子，：

```SQL
-- 将查询出得结果按照商品单价从小到大进行排序
SELECT product_id, product_name, sale_price, purchase_price
FROM Product
ORDER BY sale_price;
```

其结果为：

![order by 子句](../图片/数据库截图/order by 子句.png)

**不论何种情况， ORDER BY 子句都需要写在 SELECT 语句的末尾。这是因为对数据行进行排序的操作必须在结果即将返回时执行。 ORDER BY子句中书写的列名称为排序键。**

#### 指定升序或降序

​	上面的例子我们可以看出，是按照sale_price这一列的值从低到高进行排列的，**也可以看出，ORDER BY子句默认对所指定的列进行升序排序**，称为升序；那当然还有降序排列，**降序排列所用关键字为desc**，按照指定列的值从大到小进行排列。

```SQL
SELECT product_id, product_name, sale_price, purchase_price
FROM Product
ORDER BY sale_price DESC;
```

其查询结果为：

![降序排列](../图片/数据库截图/降序排列.png)

**尽管ORDER BY子句默认是进行升序排列的，SQL也提供了关键字来表明其升序，就是ASC，所以，ASC用来升序排列，DESC用来降序排列，两个关键字的用法是一致的。**

#### 指定多个排序键（排序列）

```SQL
-- 对商品单价进行降序排列，对商品编号进行升序排列
SELECT product_id, product_name, sale_price, purchase_price
FROM Product
ORDER BY sale_price desc, product_id ASC;
```

其结果为：

![指定多个键排序](../图片/数据库截图/指定多个键排序.png)

上面的SQL语句是指定商品单价降序排列，商品id升序排列，从图中可以看出，在单价都为500元的这两行，其id值是从0002到0006的，这说明确实对id值进行了升序排列。

​	**一般指定多个排序键的作用就是在前面的排序键中可能存在类似单价都为500的这种情况，相同了，那么就可以指定另一个排序键对这两行数据进行区分**。

#### Null的顺序

​	Null值会被怎么排序呢？下面类看一个例子：

```SQL
-- 对purchase_price这一列进行升序排序
SELECT product_id, product_name, sale_price, purchase_price
FROM Product
ORDER BY purchase_price;
```

其结果为：

![有null值的升序](../图片/数据库截图/有null值的升序.png)

**SQL语法是这样规定的，在排序键中包含null值时，包含null值的这几行总是会被放在排序开头或结尾；而在MySQL中我们可以看出把null值放在了开头。**

#### 在排序键中使用列的别名

​	在 GROUP BY 子句中不能使用SELECT 子句中定义的别名，但是在 ORDER BY 子句中却是允许使用别名的，例如：

```SQL
SELECT product_id AS id, product_name, sale_price AS sp, purchase_price
FROM Product
ORDER BY sp, id; -- 在这里就使用了前面定义的列的别名
```

其输出结果为：

![在排序键中使用别名](../图片/数据库截图/在排序键中使用别名.png)

不能在 GROUP BY 子句中使用的别名，为什么可以在 ORDER BY子句中使用呢？现在可以大致考虑一下**SQL语句的执行顺序**了：

**FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY**

可以看出，select子句在ORDER BY子句之前执行，那么select子句中定义的别名就可以被ORDER BY子句识别，但是却不能被HAVING子句识别。

#### ORDER BY子句中可以使用的列

​	ORDER BY 子句中也可以使用存在于表中、但并不包含在 SELECT子句之中的列。例如：

```SQL
SELECT product_name, sale_price, purchase_price
FROM Product
ORDER BY product_id; -- product_id这一列没有再select子句中
```

其执行结果为：

![排序键可以不使用select语句中的列](../图片/数据库截图/排序键可以不使用select语句中的列.png)

#### 不要使用列编号

```SQL
-- 通过列编号指定
SELECT product_id, product_name, sale_price, purchase_price
FROM Product
ORDER BY 3 DESC, 1;-- 3是指select子句中的sale_price，1是指product_id
```

​	列编号是指 SELECT 子句中的列按照从左到右的顺序进行排列时所对应的编号（1, 2, 3, …）。

​	**上面的这种形式可能是可以执行的，但是不推荐使用，因为可读性太差。**

## 使用数据处理函数

​	在MySQL数据库中，封装了很多的函数，但是这些函数大多都是很私有化的，并不像SQL语句那样可以在多个数据库之间通用，所以，使用它们可能会导致再换一个数据库的时候就需要修改代码。

​	**所以，在一定要使用数据库提供的函数来处理数据的时候，一定要注意做好注释，以便将来的修改。**

​	在MySQL中函数，大致可以分为这么几类：文本处理函数、日期处理函数、数值处理函数。在这里，只对日期处理函数进行详细说明。

### 日期时间处理函数

​	MySQL提供的日期时间处理函数有这么几种：

![日期时间处理函数](../图片/数据库截图/日期时间处理函数.png)

在使用这些函数之前，要先了解一下，MySQL中存储日期时间的格式是什么？**在MySQL中，存储日期的格式是yyyy-mm-dd，存储时间的格式是hh：MM:ss**，只要有一个字段日期或时间格式，那么它在插入或更新到数据库中时，它存储的格式就是这样的。

```
SELECT DATE(regist_date) FROM product -- 在这里使用了DATE函数
WHERE product_id=0001
```

从上面的SQL语句中也可以看出，函数是用在字段上的，用来对该字段中的数据按一定的格式进行处理。

## 数据库数据更新语句

### 数据的插入语句

#### INSERT INTO的基本用法

​	数据的插入语句用的是关键字INSERT INTO，其语法格式如下：

```SQL
INSERT INTO < 表名 > ( 列 1 , 列 2 , 列 3 , …… )
VALUES ( 值 1 , 值 2 , 值 3 , …… );
```

例如我们向一张表中插入一些数据：

```SQL
-- 向ProductIns表中插入一些数据
INSERT INTO ProductIns (product_id, product_name, product_type, 
sale_price, purchase_price, regist_date)
VALUES ('0001', 'T 恤衫 ', ' 衣服 ', 1000, 500, '2009-09-20');
```

执行数据更新语句返回的值就是更新成功的行数，所以不再展示结果。

**注意：表名后面的列清单和 VALUES 子句中的值清单的列数必须保持一致。**

**原则上，执行一次 INSERT 语句会插入一行数据。**

虽说原则上执行一次插入只能插入一条行记录，但是在MySQL中是支持一次插入多行记录的，例如：

```SQL
-- 通常只能插入一行记录
INSERT INTO ProductIns VALUES ('0002', ' 打孔器 ', ' 办公用品 ', 500, 320, '2009-09-11');
INSERT INTO ProductIns VALUES ('0003', ' 运动 T 恤 ', ' 衣服 ', 4000, 2800, NULL);
INSERT INTO ProductIns VALUES ('0004', ' 菜刀 ', ' 厨房用具 ', 3000, 2800, '2009-09-20');
-- 多行 INSERT （ Oracle 以外）
INSERT INTO ProductIns 
VALUES 
('0002', ' 打孔器 ', ' 办公用品 ', 500, 320, '2009-09-11'),
('0003', ' 运动 T 恤 ', ' 衣服 ', 4000, 2800, NULL),
('0004', ' 菜刀 ', ' 厨房用具 ', 3000, 2800, '2009-09-20');
```

**注意：多行 INSERT 的语法并不适用于所有的 RDBMS。该语法适用于 DB2、SQL、SQL Server、PostgreSQL 和 MySQL，但不适用于 Oracle。**

#### 列清单的省略

​	当对表进行全列 INSERT 时（全列插入代表的是按表中的所有列进行插入），可以省略表名后的列清单。这时 VALUES子句的值会默认按照表中列的顺序对应从左到右的顺序赋给每一列。	例如：

```SQL
-- 包含列清单
INSERT INTO ProductIns (product_id, product_name, product_type, 
sale_price, purchase_price, regist_date) VALUES ('0005', ' 高压锅 ', 
' 厨房用具 ', 6800, 5000, '2009-01-15');

-- 省略列清单
INSERT INTO ProductIns VALUES ('0005', ' 高压锅 ', ' 厨房用具 ', 
6800, 5000, '2009-01-15');
```

#### 插入null值

​	可以给表中的某一列插入null值，前提是该列没有进行非空约束。否则会报错。

#### 插入默认值

​	当在创建表时如果给该列设置了默认约束（也就是给该列设置了一个默认值）的话，就可以在插入时插入该默认值。

##### 显示的插入默认值

```SQL
-- 显示的对ProductIns表中的sale_price插入默认值
INSERT INTO ProductIns (product_id, product_name, product_type, 
sale_price, purchase_price, regist_date) VALUES ('0007', 
' 擦菜板 ', ' 厨房用具 ', DEFAULT, 790, '2009-04-28');
```

例如上面的插入语句中使用了default关键字，就代表给该列插入一个默认值。

##### 隐式的插入默认值

​	如果该列设置了默认值的话，那么在采用隐式的插入默认值的方式插入语句的时候，只要省略了该列对应的插入的值就可以了，连default关键字都不用写，数据库会默认将该列的默认值放到这里。

##### 未设置默认值

​	当未设置默认值，但又采用了这种插入默认值的方式的时候，默认就会插入一个null值。

#### 从其它表中复制数据

​	从其它表中复制数据，采用的是INSERT...SELECT语句，例如：

```SQL
-- 将商品表中的数据复制到商品复制表中
INSERT INTO ProductCopy (product_id, product_name, product_type, 
sale_price, purchase_price, regist_date)
SELECT product_id, product_name, product_type, sale_price, 
purchase_price, regist_date
FROM Product;
```

**注意，在采用这种方式的时候，尽量保持原表与新表的结构是一致的，这样在复制的时候不会因为数据类型等错误而导致失败。**

​	INSERT 语句的 SELECT 语句中，可以使用 WHERE 子句或者 GROUP BY 子句等任何SQL语法 （但使用 ORDER BY 子句并不会产生任何效果）。

### 数据的删除语句

#### DROP TABLE 语句和 DELETE 语句	

- DROP TABLE 语句可以将表完全删除
- DELETE 语句会留下表（容器），而删除表中的全部数据

​        DROP TABLE 语句会完全删除整张表，因此删除之后再想插入数据，就必须使用 CREATE TABLE 语句重新创建一张表。

​	DELETE 语句在删除数据（行）的同时会保留数据表，因此可以通过 INSERT 语句再次向表中插入数据。	

#### DELETE 语句的基本语法

其基本的语法格式为：

```SQL
DELETE FROM < 表名 >;
```

​	执行使用该基本语法的 DELETE 语句，会删除指定的表中的全部数据行。

**一般我们不用这种方式，太危险了。**

#### 指定删除对象的 DELETE 语句

​	想要删除部分数据行时，可以像 SELECT 语句那样使用 WHERE子句指定删除条件。

其语法格式为：

```SQL
DELETE FROM < 表名 >
WHERE < 条件 >;
```

```SQL
-- 删除售价大于等于4000元的商品
DELETE FROM Product
WHERE sale_price >= 4000;
```

**DELETE 语句中只能使用 WHERE 子句。**

####  TRUNCATE 语句

​	在标准的SQL语句中，只有DELETE语句可以删除表中的数据，但是各个数据库系统又提供了另一种方式，用来代表舍弃表中数据，例如 Oracle、SQL Server、PostgreSQL、MySQL 和 DB2等数据库中就都提供了TRUNCATE 语句，其格式为：

```SQL
TRUNCATE < 表名 >;
```

​	与 DELETE 不同的是， TRUNCATE 只能删除表中的全部数据，而不能通过WHERE 子句指定条件来删除部分数据。也正是因为它不能具体地控制删除对象，所以其处理速度比 DELETE 要快得多。实际上， DELETE 语句在 DML 语句中也属于处理时间比较长的，因此需要删除全部数据行时，使用 TRUNCATE 可以缩短执行时间。	

#### DELETE语句和TRUNCATE语句的区别与联系

- 如果delete语句后不加条件，单从效果上看，两个语句是一样的，都会清空表中的数据。
- delete是DML类型的语句；而truncate是DDL类型的语句，执行truncate需要drop权限，而且由于它是DDL语句，是通过删除表，然后再重建实现的，所以**执行的时候要比delete性能高很多**，因为不需要一行一行的去删除数据。
- delete 过程如果出现错误，事务是可以回滚的，但是**truncate操作时是不会造成回滚的**，因此更需要小心，所以才需要授予drop的权限。
- 从执行效果来看：
  - delete的使用范围更广，因为它可以删除符合条件的数据行，而不一定是整体；但是truncate只能删除整体。
  - delete会返回删除数据的行数，但是truncate只会返回0，这个没有任何意义。
  - 在存储引擎为InnoDB的数据库服务器中，truncate无法删除一个表中的数据，如果这个表的主键作为了另一个表的外键，无论这个外键是否存在，这是由于drop表限制的；但是delete是可以实现，只要的这个主键在另一个表中的外键不存在即可。
  - 在InnoDB中，对于AUTO_INCREMENT的列来说，当在truncate之后，改列的第一个值仍从1开始，但是delete仍然使用顺序使用删除之前的值。

### 数据的更新语句

#### UPDATE 语句的基本语法

​	UPDATE语句用来更新数据信息，其基本格式为：

```SQL
UPDATE < 表名 >
SET < 列名 > = < 表达式 >，< 列名 > = < 表达式 >......
```

**注意，这样的话会将所有行记录中该列的值修改掉，这样也是不可取的**。所以我们一般用下面的格式：

```SQL
UPDATE < 表名 >
SET < 列名 > = < 表达式 >，< 列名 > = < 表达式 >...
WHERE < 条件 >;
```

用where子句来指定要更改的是哪几行数据，这样比较安全。

#### 使用Null进行更新

​	使用 UPDATE 也可以将列更新为 NULL （该更新俗称为 NULL 清空）。此时只需要将赋值表达式右边的值直接写为 NULL 即可。

## 子查询

​	子查询就是嵌套在查询语句中的其它查询。

### 使用子查询进行过滤

​	我们来看一个子查询的语句：

```SQL
SELECT cust_id
FROM orders
WHERE order_num IN (SELECT order_num 
		    FROM orderitems
		    WHERE prod_id = 'TNT2')
```

分析一下上面的子查询语句。**在SELECT语句中，子查询总是从内向外处理的。**

​	在执行上面的查询语句的时候，首先执行的是SELECT order_num   FROM orderitems  WHERE prod_id = 'TNT2'这一条查询语句，这个查询的意思是，从orderitems表中根据prod_id = 'TNT2'查询出其 order_num订单号，查询出得订单号为20005和20007，然后把这两个订单号作为外查询的查询条件。

​	执行完内查询后，才会执行外查询，此时外查询变成了SELECT cust_id   FROM orders  WHERE order_num IN （20005,20007）；外查询执行的是这条语句，从orders表中查出cust_id，查询条件order_num IN （20005,20007）。

​	其实子查询就是将内查询的结果作为外查询的条件。

**在子查询中，是允许继续嵌套子查询的，也就是可以有无限个子查询语句，**例如：

```SQL
SELECT cust_name,cust_contact
FROM customers
WHERE cust_id IN (SELECT cust_id
		  FROM orders
		  WHERE order_num IN (SELECT order_num
				      FROM orderitems
				      WHERE prod_id = 'TNT2'))
```

在上面的查询语句中就有两个子查询语句。**同样，在查询处理的时候也是会先执行内层的查询语句，然后才会执行外层的查询语句。**

​	**子查询一般与IN操作符相结合使用，但是也可以使用比较运算符等。具体情况具体看查询的条件是什么。**

### 作为计算字段使用子查询（标量子查询）

看一个例子：

```SQL
SELECT cust_name,
       cust_state,
       (SELECT COUNT(*)
		FROM orders
		WHERE orders.cust_id = customers.cust_id) AS orders
FROM customers
ORDER BY cust_name;
```

其查询结果为：

![子查询](../图片/数据库截图/子查询.png)

在这个查询语句中，子查询作为了一个外层查询的字段，用来统计每个用户的订单数是多少。

​	而在这里的内查询的执行顺序就有点不同了，它先是执行的外层查询，检索出一个客户后，再去拿这个客户的id去执行内查询。

**所以，子查询的执行顺序并不总是先内查询然后外查询，内查询的执行语句要看它所在子句的执行顺序。到了该执行该子句的时候，它里面的内查询也会被执行。**

### 使用子查询要注意的几个点

- 标量子查询有一个特殊的限制，那就是必须而且只能返回 1 行 1列的结果，	也就是返回一个单一的、确定的值。
- 标量子查询的返回值可以用在 = 或者 <> 这样需要单一值的比较运算符之中。
- 普通的子查询可能返回多个值，一般用IN操作符，但是不能用在 = 或者 <> 这样需要单一值的比较运算符之中。
- 能够使用常数或者列名的地方，无论是 SELECT 子句、 GROUP BY 子句、 HAVING 子句，还是ORDER BY 子句，几乎所有的地方都可以使用。
- 关联条件一定要写在子查询中，例如，orders.cust_id = customers.cust_id这个关联条件，这是因为，在外层查询中根本就没有orders表，这里遵循的是“内部能看见外部，外部看不见内部”的原则。

## 表的集合运算

### 表的加减法

#### 表的加法（UNION）

​	准备了两张表，product表和product表2，来对两张表进行加法运算。

```SQL
SELECT product_id, product_name
FROM Product
UNION
SELECT product_id, product_name
FROM Product2;
```

其结果为：

![union运用](../图片/数据库截图/union运用.png)

我们来看一下对UNION运用的说明：

![图解UNION运用](../图片/数据库截图/图解UNION运用.png)

​	商品编号为“ 0001 ”~“ 0003 ”的 3 条记录在两个表中都存在，但是在经过运算之后只留下了一组这3条记录，没有将这三条记录进行重复显示，这是因为 UNION 等集合运算符通常都会除去重复的记录。

#### 集合运算的注意事项

##### 作为运算对象的记录的列数必须相同

​	例如，像下面这样，一部分记录包含 2 列，另一部分记录包含 3 列时会发生错误，无法进行加法运算。	

```SQL
-- 列数不一致时会发生错误
SELECT product_id, product_name
FROM Product
UNION
SELECT product_id, product_name, sale_price
FROM Product2;
```

##### 作为运算对象的记录中列的类型必须一致

​	从左侧开始，相同位置上的列必须是同一数据类型。例如下面的 SQL语句，虽然列数相同，但是第 2 列的数据类型并不一致（一个是数值类型，一个是日期类型），因此会发生错误 。

```SQL
-- 数据类型不一致时会发生错误
SELECT product_id, sale_price
FROM Product
UNION
SELECT product_id, regist_date
FROM Product2;
```

##### 可以使用任何 SELECT 语句，但 ORDER BY 子句只能在最后使用一次

​	通过 UNION 进行并集运算时可以使用任何形式的 SELECT 语句，之前学过的 WHERE 、 GROUP BY 、 HAVING 等子句都可以使用。但是ORDER BY 只能在最后使用一次。

```SQL
SELECT product_id, product_name
FROM Product
WHERE product_type = ' 厨房用具 '
UNION
SELECT product_id, product_name
FROM Product2
WHERE product_type = ' 厨房用具 '
ORDER BY product_id;
```

#### 包含重复行的集合运算（ALL选项）

​	使用ALL关键字可以在UNION的结果中保留重复的行。

```SQL
SELECT product_id, product_name
FROM Product
UNION ALL
SELECT product_id, product_name
FROM Product2;
```

其结果为：

![UNION ALL用法](../图片/数据库截图/UNION ALL用法.png)

如图所示，对比上面的结果，可以看出id值为0001~0003的记录重复出现了。

#### 选取表中公共部分（INTERSECT在MySQL中不支持）

​	选取两个记录集合中公共部分使用关键字 INTERSECT （交集）。

```SQL
SELECT product_id, product_name
FROM Product
INTERSECT
SELECT product_id, product_name
FROM Product2
ORDER BY product_id;
```

#### 记录的减法（EXCEPT在MySQL中也不支持）

​	EXCEPT用来对两张表进行减法。

```SQL
SELECT product_id, product_name
FROM Product
EXCEPT
SELECT product_id, product_name
FROM Product2
ORDER BY product_id;
```

​	只有Oracle不使用 EXCEPT ，而是使用其特有的 MINUS 运算符。**在MySQL中是不支持这个操作的。**	

### 表之间的联结

​	这里要注意的是，前面使用的UNION集合运算是导致行记录的增加，而接下来的联结运算是导致列记录的增加。

#### 内联结—— INNER JOIN

​	我们创建了两张表，一张是product表，另一张是shopProduct表，两张表并没有建立外键约束，但是两张表中都有product_id这一列，以其来模拟外键约束。如图所示：

![两张表联结查询](../图片/数据库截图/两张表联结查询.png)

来将两张表进行联合：

```SQL
SELECT SP.shop_id, SP.shop_name, SP.product_id, P.product_name, 
P.sale_price
FROM ShopProduct AS SP INNER JOIN Product AS P ON 
SP.product_id = P.product_id;
```

- **内联结要点1：** FROM 子句中我们使用了INNER  JOIN......ON将两张表联结到了一起，从查询结果中也可以看出，查询出得内容是两张表中的内容。

  ![内联结查询](../图片/数据库截图/内联结查询.png)

  ShopProduct AS SP这句语句的意思是我们为ShopProduct这张表起了一个别名为SP，同样，也可以为相同的方式为表中的列起别名。但是在oracle数据库中是不能使用AS关键字的，会报错，所以在oracle数据库中直接省略AS关键字就可以了，表名后面直接跟表别名，例如ShopProduct  SP。

- **内联结要点2：**ON子句用来指定两张表之间的关系，一般就是一个表的外键是另一个表的主键，虽说两张表并没有建立真正的外键约束，但是也是可以模拟外键约束的。需要指定多个键时，同样可以使用 AND 、 OR 。在进行内联结时 ON 子句是必不可少的（如果没有 ON会发生错误），并且 ON 必须书写在 FROM 和 WHERE 之间。

  对于ON子句中指定的条件就像图中所示那样，将两张表联结了起来：

  ![内联结中的ON子句](../图片/数据库截图/内联结中的ON子句.png)

  联结条件也可以使用“ = ”来记述。在语法上，还可以使用 <= 和BETWEEN 等谓词。但因为实际应用中九成以上都可以用“ = ”进行联结，所以开始时大家只要记住使用“ = ”就可以了。使用“ = ”将联结键关联起来，就能够将两张表中满足相同条件的记录进行“联结”了。

- **内联结要点3：**在 SELECT 子句中，像 SP . shop _ id 和 P . sale _ price 这样使用“ < 表的别名 > . < 列名 > ”的形式来指定列。和使用一张表时不同，由于多表联结时，某个列到底属于哪张表比较容易混乱，因此采用了这样的防范措施。

##### 内联结和where子句结合使用

​	内联结还可以和where子句结合来使用，通过where条件来限定去除一部分的内容。

```SQL
SELECT SP.shop_id, SP.shop_name, SP.product_id, P.product_name, 
P.sale_price
FROM ShopProduct AS SP INNER JOIN Product AS P ON 
SP.product_id = P.product_id
WHERE SP.shop_id = '000A';
```

其结果为：

![内联结和where子句联合使用](../图片/数据库截图/内联结和where子句联合使用.png)

在上面的查询语句中，就用where子句做了限定，SQL执行顺序也是先是from子句，然后是where子句，最后才是select子句。

​	像这样使用联结运算将满足相同规则的表联结起来时， WHERE 、GROUP BY 、 HAVING 、 ORDER BY 等工具都可以正常使用。我们可以将联结之后的结果想象为新创建出来的一张表	，当然，这张“表”只在 SELECT 语句执行期间存在， SELECT 语句执行之后就会消失。

#### 外连接—— OUTER JOIN

​	外联结也是通过 ON 子句的联结键将两张表进行联结，并从两张表中同时选取相应的列的。	

```SQL
SELECT SP.shop_id, SP.shop_name, SP.product_id, P.product_name, 
P.sale_price
FROM ShopProduct AS SP RIGHT OUTER JOIN Product AS P 
ON SP.product_id = P.product_id;
```

上面使用了RIGHT OUTER JOIN关键字，采用了外连接，其结果为：

![外联结](../图片/数据库截图/外联结.png)

- **外连接要点1：**对比上面的结果和内联结的结果可以看出，外联结比内联结多了两条记录，这是由于内联结和外联结作用的不同造成的：

  **内联结只能选取两张表中都存在的数据，也就是在主表（INNER之前的那张表）中存在的数据才会去查子表，否则即使子表中有，而主表中却没有与之关联的信息的话，那么是不会被查询出来的。**

  **相反，对于外联结来说，只要数据存在于某一张表（主表）当中，就能够读取出来。**

- **外联结的要点2：**外联结特别要注意主表这一特征，应为外联结分为**左外联结**和**右外联结**。

  - **左外联结（LEFT JOIN）：**join左边的表是主表

    ```SQL
    表A LEFT JOIN 表B ON 表A.id=表B.aid
    -- 在这里，表A是主表，就是A表中的数据总会查出，B表中没有与A表中关联的信息的时候就用null补齐
    ```

  - **右外联结（RIGHT JOIN）：**join右边的表的主表

    ```SQL
    表A RIGHT JOIN 表B ON 表A.id=表B.aid
    -- 在这里，表B是主表，主表有的记录一定会被查出来，次表没有的话就null补齐
    ```

#### 多表联结

​	内联结和外联结通常都不单单用于两张表，往往会用于多张表中。

```SQL
SELECT SP.shop_id, SP.shop_name, SP.product_id, P.product_name, 
P.sale_price, IP.inventory_quantity
FROM ShopProduct AS SP 
INNER JOIN Product AS P  -- 多表联结是只要一直加联结就可以了
ON SP.product_id = P.product_id
INNER JOIN InventoryProduct AS IP 
ON SP.product_id = IP.product_id
WHERE IP.inventory_id = 'P001';
```

在这种多表的联结过程中，都是按照从前到后的顺序来联结表的，其执行过程是先拿最开始的两张表联结，然后联结后的结果作为一张表再与第三张表进行联结，一次类推，联结的表没有限制，只要找准表与表之前的联结关系就可以了。

## MySQL中的数据类型

### MySQL中的数值类型

​	MySQL中支持的数值类型包括如下所示：

![MySQL中的数值类型](../图片/数据库截图/MySQL中的数值类型.png)

### MySQL中支持的字符串类型

![MySQL中的字符串类型](../图片/数据库截图/MySQL中的字符串类型.png)

![MySQL中的字符串类型2](../图片/数据库截图/MySQL中的字符串类型2.png)

### MySQL中的日期类型

![MySQL中的日期类型](../图片/数据库截图/MySQL中的日期类型.png)

## 使用存储过程

### 存储过程

​	存储过程是在MySQL5版本之后才被支持的。**存储过程就是为以后的使用而保存的一条或多条MySQL语句的集合。**

​	

## MySQL数据库体系结构即存储引擎

### 定义数据库和实例

​	在数据库领域中，有两个词是容易混淆的，这就是**数据库**和**实例**。

- **数据库：**物理操作系统文件或其他形式文件的集合。在MySQL中，数据库文件可以是frm、MYD、MYI、ibd等，也可以是存储在内存中的文件。
- **实例：**MySQL数据库由后台线程以及一个共享内存区域组成。共享区域可以被运行的后台线程所共享。**数据库实例是用来操作数据库文件的。**

在MySQL数据库中，数据库与数据库实例是一一对应的，即一个实例对应一个数据库。但是在集群环境下，可能存在一个数据库被多个数据实例使用的情况。

​	MySQL采用的是单进程多线程数据架构。Oracle数据库是多进程多线程架构的（在Windows版本下是单进程多线程的）。

​	MySQL数据库实例在系统上的表现就是一个进程。

​	**数据库是文件的集合，是依照某种数据模型组织起来并存放于二级存储器中的数据集合。**

​	**数据库实例是程序，是位于用户和操作系统之间的一层数据管理软件。用户只有通过数据库实例才能和数据库打交道。**

​	下面是MySQL的体系架构图：

![MySQL的体系结构](../图片/数据库截图/MySQL的体系结构.png)

从图中可以看出，MySQL由如下几部分组成：

- 连接池组件
- 管理服务和工具组件
- SQL接口组件
- 查询分析器组件
- 优化器组件
- 缓冲组件
- 插件式存储引擎
- 物理文件

**需要注意的是，存储引擎是基于表的，而不是基于数据库的。**

### MySQL存储引擎

​	MySQL区别于其它数据库的最重要的一个特点就是：**MySQL的插件式的表存储引擎。**MySQL数据库的核心在与存储引擎。

​	MySQL由于是开源的，所以其所用的存储引擎可以分为MySQL官方存储引擎和第三方存储引擎。有些第三方存储引擎很强大，例如InnoDB存储引擎，在被Oracle收购之前就是很强大的存储引擎。

#### InnoDB存储引擎

​	InnoDB存储引擎支持事务，其设计的目标主要面向在线事务（OLTP）处理的应用。其特点是行锁设计、支持外键、并支持类似于Oracle的非锁定读，即默认读取操作不会产生锁。从MySQL数据库5.5.8版本开始，InnoDB存储引擎是默认的存储引擎。

​	**InnoDB存储引擎的主要趋势有如下几个：**

- **InnoDB存储引擎支持事务操作，实现了SQL标准的四种隔离级别。**
- **支持行锁设计，支持类似于Oracle数据库的一致性非锁定读，使用多版本控制（MVCC）来获得高并发性。**
- **InnoDB存储引擎采用聚簇索引的方式，来优化数据库的主键查询。**
- **在物理上支持外键约束，也就是 FOREIGN KEY约束，来保证数据的完整性。（在MyISAM存储引擎上就不支持）**
- **InnoDB存储引擎使用间隙锁（next-key locking）的策略来避免幻读现象的产生。**

​	InnoDB存储引擎将数据放在一个逻辑表空间中，这个表空间就像一个黑盒一样由InnoDB存储引擎自身进行管理。

​	InnoDB通过使用多版本并发控制来获得高并发性，并且实现了SQL标准的四种隔离级别，默认为可重复读级别。同时，使用一种被称为间隙锁（next-key locking）的策略来避免幻读现象的产生。

![InnoDB存储引擎的特点](../图片/数据库截图/InnoDB存储引擎的特点.png)

​	对于表中数据的存储，InnoDB采用了聚集的方式。因此每张表的存储都是按主键的顺序进行存放的，如果没有在表定义时显示的指定主键，那么他会为每一行生成一个6个字节的ROWID，以此作为主键。

​	InnoDB是一个高性能、高可用、高可扩展的存储引擎。

#### MyISAM存储引擎

​	该存储引擎不支持事务，采用表锁设计，支持全文索引，主要面向一些联机分析处理（OLAP）数据库应用。在MySQL5.5.8之前它是默认的存储引擎（除Windows版本之外）。

​	该存储引擎不同于InnoDB存储引擎的两个方面：

- **不支持事务**
- **它的缓冲池只缓存索引文件，而不缓存数据文件。**
- **支持全文搜索索引**
- **锁定粒度为表级**

​       MyISAM存储引擎表由MYD和MYI组成，MYD用来存放数据文件，MYI用来存放索引文件。

​	在MySQL5.0之前，该存储引擎默认支持的表大小为4GB，之后改为了256TB。

![MyISAM存储引擎注意点](../图片/数据库截图/MyISAM存储引擎注意点.png)

#### NDB存储引擎

​	NDB存储引擎是一个集群存储引擎，类似于Oracle的RAC集群。但是它比RAC集群能提供更高的可用性。

​	NDB的特点是它将数据全部存放在内存中，因此主键查找的速度是很快的。并且通过添加NDB数据存储节点可以线性地提高数据库性能，是高可用、高性能的集群系统。（从MySQL5.1版本开始，它允许将非索引数据文件放在磁盘上）

​	NDB存储引擎是支持行级锁定的。

​	NDB存储引擎有一个缺点：那就是它的联结（JOIN）操作是在MySQL数据库层完成的，而不是在存储引擎层完成的。这意味着复杂的联结操作需要巨大的网络开销，因此查询速度很慢。这是制约其使用的一大缺点。

#### Memory存储引擎

​	Memory存储引擎（也叫HEAP存储引擎）将表中的数据存放在内存中。如果数据库重启或发生崩溃，表中的数据将全部消失。所以它非常适合存储**临时表的临时数据**，以及数据仓库中的纬度表。

​	Memory存储引擎的索引默认使用哈希索引，而不是常用的B+数索引，但是也支持B树索引。

​	Memory存储引擎的的优点就是查询速度快。

​	它的缺点也很明显：只支持表锁，并发性能差，并且不支持TEXT和BLOB类型。同时，他对于varchar变长数据类型数据的存储也是按char类型来定长存储的，因此浪费资源。

#### Archive存储引擎

![Archive存储引擎](../图片/数据库截图/Archive存储引擎.png)

#### Federated存储引擎和Maria存储引擎

![Federated和Maria存储引擎](../图片/数据库截图/Federated和Maria存储引擎.png)

#### 关于MySQL数据库常见的几个误解

- MySQL数据库不支持全文索引：这是不对的，MyISAM、InnoDB存储引擎等都支持全文索引。
- MySQL数据库连接速度快是因为不支持事务：这是错的，MySQL的MyISAM存储引擎不支持事务，但是InnoDB存储引擎是支持的，不同的场景需要不同的存储引擎，各自有各自的优点。
- 当表的数据量大于1000万是MySQL的性能会急剧下降吗？不是的，不正确的选择存储引擎和不正确的配置才是导致MySQL的性能急剧下降的罪魁祸首。

## MySQL中的索引

​	索引有很多种类型，在MySQL中，索引是在存储引擎层而不是服务器层实现的，所以，在MySQL中，索引没有统一的标准，它主要是根据存储引擎的不同采取不同的索引。并不是所有的存储引擎都支持所有的索引。下面来看一下在MySQL中常见的索引类型。

### 为什么要创建索引

​	使用索引在大部分的时候，都能够帮我们在数据库中很快的找出我们想要的值。

- 使用索引，可以加快对WHERE子句匹配的行进行搜索的速度；或者加快对与另一个连接表里的行匹配的行的搜索速度。
- 对于使用MIN()或MAX()函数的查询，MySQL可以在不用逐行检查的情况下快速找到索引列里的最小值或最大值。
- 对于ORDER BY和GROUP BY子句，MySQL经常使用索引来高效的完成分类和分组操作。

### 索引总是好的吗？

​	索引总是好的嘛？不是的，索引也有缺点的：

- 索引在加快检索的同时，也可能会降低索引列插入、删除和更新值的速度。也就是说索引在大部分情况下会降低写入操作的速度。这是因为在执行写入操作的时候，不光光只是插入数据行，还要更改索引表，在索引数据很多的时候，效率下降很多。
- 索引会占用磁盘空间，在有大量的索引的时候，会占用大量的磁盘空间。

### 什么情况下会放弃索引进行全表扫描

- **使用IN操作符会导致放弃索引**
- **不等于操作会导致放弃索引**
- **IS NULL 或IS NOT NULL操作（判断字段是否为空）** 
- **LIKE操作符** ：在like中，将通配符放在匹配条件的前面会导致放弃索引
- **最左索引要正确运用，查询条件要写的适当**

### 索引创建的语法

​	MySQL提供了多种灵活的索引创建办法。

- 可以对单个列或多个列建立索引，多列所以呢也被称作复合索引。

- 索引可以只包含一个值，也可以包含重复值。

- 可以为同一个表创建多个索引，帮助优化对表的不同类型的查询。

- 对于MySQL中的某些数据类型，可以为其创建前缀索引，而不必为整个列创建索引，这样可以使得索引更小，加快查询索引的速度。（对于BLOB和TEXT两种数据类型的列，只可以对其建立前缀索引）

  ![存储引擎索引的特性](../图片/数据库截图/存储引擎索引的特性.png)

#### 创建索引

​	MySQL可以创建多种类型的索引：

- **主键索引**：根据表中的主键值来建立的索引，索引名一定是primary，非空唯一。
- **唯一索引**：对于单列索引，不允许有重复的索引值出现；对于多列索引（复合索引）不允许出现重复的组合值。允许多个null值。
- **联合索引：**通过表中的多个列来建立索引，而不只只是采用一个列来创建索引。
- **常规（非唯一性）索引**：可以出现重复值。
- **FULLTEXT（全文索引）索引**：它用于完成全文索引，这种类型在MySQL5.6.4版本之前只适用于MyISAM存储引擎；在这个版本之后亦可以用于InnoDB存储引擎上。
- **SPATIAL索引（空间索引）**：这种索引只适用于包含空间值的MyISAM存储引擎。
- **HASH索引**：这是MEMORY存储引擎的默认索引，在NDB存储引擎中也是支持hash索引的。而如果要使MyISAM和InnoDB引擎也支持的话，可以通过伪哈希索引来实现，叫自适应哈希索引。主要通过增加一个字段，存储hash值，将hash值建立索引，在插入和更新的时候，建立触发器，自动添加计算后的hash值到表里。

MySQL中创建索引的方式有三种：

- 在创建表的时候就指定索引列；
- 采用CREATE  INDEX语句；
- 采用ALTER  TABLE语句。

##### 在创建表时创建索引

```MySQL
create table product3( -- 创建一张表
  id int,
  age int,
  username varchar(125),
  address varchar(124),
  phone_num varchar(255),
  password varchar(254),
  -- 上面是列的定义

  primary key id_index (id),   --  这是在定义一个主键索引，索引的名字不管指定没有都是primary，所以只能存在一个
    
  unique username_index (username（20）), -- 创建一个唯一索引，username_index是索引名，括号中指定要创建索引的列名，列名括号中的20代表前缀长度
    
  index (id,age), -- 创建普通复合索引，没有指定索引名，默认以第一个列名id为索引，在检索该索引时先检索id然后再检索age
    
  fulltext (phone_num)-- 创建索引全文索引，没有指定索引名
    
  -- spatial (password) -- 这是在创建空间索引，在合适的引擎中才可以创建

)engine = MyISAM; -- 指定存储引擎为MyISAM，在MySQL5.5中InnoDB是不支持全文索引的
```

**要注意的是，在列定义的时候，我们可能会在某个列定义后加上primary key用来标识该列为主键列，或者加上unique约束表明该列的值唯一，而在这种时候，MySQL就自动为其生成了索引，主键索引和唯一索引**。

​	在对上面的表查询索引后的结果如下：

![索引查询](../图片/数据库截图/索引查询.png)

可以看出大部分索引都是BTREE索引，只有全文索引是fullText索引类型。同时，我们可以看出，在索引名中有两个id，**这不是两个索引**，在Seq_in_index这一列中可以看出，id次序为1，而age次序为2,所以**联合索引是有次序问题的**。在索引的创建中应该注意以下几个点：

- **同一个表中不允许有同名索引的存在；**
- **主键索引的索引名一定是primary；**
- **主键索引的列不可以包含null值**，而unique类型的索引代表的列可以有null值，而且null值可以有多个（因为NULL值不会等于任何一个值），但是其它实际的值不能重复，因为该列的约束是unique（唯一）的。

##### 用ALTER  TABLE语句创建索引

​	用ALTER  TABLE语句可以创建任何一种索引，其创建的格式大体上同上，创建复合索引就在括号中用逗号分隔开多个列：

![alter table创建索引](../图片/数据库截图/alter table创建索引.png)

同时，在一个ALTER  TABLE语句中可以有多个ADD子语句，就像创建表时定义多个列一样，简化了操作，也可以创建复合索引。

##### 用CREATE  INDEX语句创建索引

​	用CREATE  INDEX语句可以创建除了主键索引之外的其它索引。

![create index创建索引](../图片/数据库截图/create index创建索引.png)

**该语句与ALTER  TABLE语句创建索引不同的是：该语句中创建索引时索引名是必须的，该语句中不能包含多个on自语句来创建多个索引，可以创建复合索引。**

##### 索引的删除

​	如果要删除某个索引的话，就可以使用DROP  INDEX语句或者ALTER  TABLE语句。

```SQL
DROP INDEX index_name on table_name;
ALTER TABLE table_name DROP index_name;
```

### 什么样的索引比较好

- **为用于搜索、排序或分组的列创建索引，而对于用于做输出显示的列则不用创建索引；**
  - 一般就是出现在WHERE子句中的列、联结子句中的列、或者是出现在ORDER BY子句或者GROUP  BY子句中的列创建索引。
  - 而对于仅仅只出现在select子句中的列就不需要创建索引了。
  - 对于出现在联结子句中或者WHERE子句中的col1=col2这种两列进行相关操作的列上建立所以是很好的选择。
- **认真考虑数据列的基数。**
  - 数据列的基数就是该列中不重复的数据有几种；例如一个列中的数据为1,2,5,4,2,4,7，那么这个列中的基数就是排除重复值后的数据就是5.
  - 列的基数越高（也就是该列中唯一值多，重复值少时）是很好的索引列。

- **索引短小值；**
  - 尽量选用较小的数据类型，例如该列的值的长度都不会超过25个字符时，那么就不要对该列的数据类型定义使用char（100），应该限定的更小，例如char（30）。
  - 短小的值可以让比较操作的速度加快，从而加快索引查找速度。
  - 短小值可以让索引短小，从而减少对磁盘I/O的请求。
  - 对于短小的键值可以使MySQL在一次性的情况下读取更多的键值到内存中，这样减小的了对I/O的请求，同时增加了在内存中索引的键值，查询出数据的几率被大幅度提高。
  - 对于InnoDB存储引擎，它采用的是聚簇索引，即将主键值和数据行存储在一起；而其他的存储引擎的索引都是二级索引，即它们把主键值和二级索引存放在一起，根据二级索引查询到主键然后再定位到数据行，这意味着主键值会重复的出现，如果主键值较大时，会占用更多的存储空间。

- **索引字符串的前缀值。**想要对字符串列进行索引，应当尽可能指定前缀长度。
  - 有一个char（200）列，在假如该列的大多数值的前10或20个字符的组合是唯一的，那就不用为整个列建立索引，而是在建立索引的时候指定其前缀长度来建立索引，这样相当于使用了前面的索引短小值的好处。

- **利用最左前缀。**假设有一个表的复合索引是Country/state/city，在索引中，列的排列顺序就是这样的，因此，数据库在对这一复合索引的相关值进行排序的时候，会优先按照Country/state进行排序，然后才是Country排序。这意味着在查询中必须指定Country值，其它值可以有也可以没有，它们都会根据索引去查询，但是如果在查询条件中只有state和city值没有Country值时，此时无法通过索引来进行筛选。
- **不要建立过多的索引**。索引要适量，且建立真正需要的索引，而不是一味的为所有列建立索引。索引的维护和空间消耗都是一种负担。
- **要选择合适的索引类型来进行比较操作。**InnoDB默认为BTREE索引；MyISAM也会使用BTREE索引，但是当该列为空间值时会使用RTREE索引；MEMORY引擎默认使用hash索引。
  - 对于散列索引，在执行精确匹配时，例如等于或者不等于比较操作时是很快的。但是当对其进行范围比较时采用哈希类型的索引是很慢的。例如大于、小于或者between...and等操作。
  - 在进行精确比较或者范围比较时，使用BTREE类型的索引总是高效的。

- **利用慢查询日志来找出那些性能低劣的查询操作。**
- **选择利于高效查询的数据类型。**
  - 多用数字运算，少用字符串运算。
  - 当较小类型够用时，就不用较大类型。
  - 把数据声明成NOT  NULL，当然在索引列上的数据最好不是null值，其它列可以随意.
  - 考虑使用枚举数据类型

### B-Tree索引

​	大多数的MySQL存储引擎都支持这种索引，即B-Tree索引，而即使称其为B-Tree索引，但是在存储引擎层面可能其实现也是不同的。例如NDB集群存储引擎内部实际上使用了T-Tree结构存储。而其表面名字也是BTREE；InnoDB则使用的B+Tree索引。

​	存储引擎使用不同的方式使用B-Tree索引，例如：

- 在InnoDB中，其按照原数据格式进行存储，根据主键引用被索引的行。
- 在MyISAM中，使用前缀压缩技术使得索引更小，其索引通过数据的物理位置引用被索引的行。

B+树索引在数据库中有一个特点是高扇出性，因此在数据库中B+树的高度一般都在2~4之间，也就是说查找某一键值的行记录时只需要2到4次I/O操作就可以了。

​	**数据库中的B+树索引分为聚集索引（聚簇索引）和辅助索引（二级索引）**。但是不管是聚集索引还是二级索引，在B+树索引中，其都是在叶子节点存放着所有的数据。而聚集索引与二级索引唯一的区别是，叶子结点存放的是否是一整行的信息。

#### 聚集索引

​	InnoDB存储引擎表是索引组织表，即表中数据按照主键（primary key）顺序存放。而聚集索引就是按照每张表的主键构造一颗B+树，同时叶子节点中存放的就是整张表的行记录数据，也将聚集索引的叶子结点称为数据页。

​	由于实际的数据页只能按照一颗B+树进行排序，因此每张表只能拥有一个聚集索引。在多数情况下，查询优化器倾向于采用聚集索引，因为其在找到索引的时候也就直接找到了数据。**聚集索引能够特别快的访问针对范围的查询**。

#### 辅助索引（二级索引）

​	对于辅助索引（非primary索引的其它索引），叶子节点并不包含行记录的所有数据，叶子节点除了包含键值以外，每个叶子节点中的索引还包含了一个书签，该书签用来告诉InnoDB存储引擎哪里可以找到对应的数据。由于InnoDB存储引擎表是索引组织表，因此**InnoDB存储引擎的辅助索引书签就是相应的行数据的聚集索引键**。

### 哈希索引

​	我们在前面知道，memory存储引擎默认的索引类型就是哈希索引，在NDB存储引擎中也支持哈希索引，它很适合于精确的比较查询，而不适合于范围查询。但是在InnoDB中，我们是无法指定建立索引时索引的类型采用hash索引的，InnoDB存储引擎支持自适应的哈希索引，采用该索引类型是由存储引擎自己掌握的，无法人为干预。

​	在InnoDB存储引擎中，它的自适应哈希索引与其它存储引擎的哈希索引一样，采用哈希表的形式来维持哈希索引，它适用的范围也就是精确的比较操作，例如等于和不等于操作，而不适用于范围查询。

### 全文检索

​	全文检索是将存储于数据库中整本书或整篇文章中的任意信息查找出来的技术。它可以根据需要获得全文中有关章、节、段、句、词等信息，也可以进行各种统计和分析。从InnoDB1.2版本开始，其支持全文索引，也就是MySQL5.6.4版本开始，以前是不支持全文索引的。

​	**全文检索通常使用倒排索引的方式来实现。**

#### 倒排索引

​	倒排索引同B+树索引一样，也是一种索引结构。它在一个辅助表中存储了单词与单词自身在一个或多个文档所在位置之间的映射。

​	**什么是倒排索引？**

​	简单理解来说，倒排索引就是**从单词检索出文档**。

​	**由于不是由记录来确定属性值，而是由属性值来确定记录的位置，因而称为倒排索引(inverted index)。**

​	其通常有两种表现形式：

- inverted   file   index：其表现形式为{单词，单词所在的文档ID}
- full   inverted   index：其表现形式为{单词，（单词所在文档ID，在该文档中的位置）}

![inverted file index存储](../图片/数据库截图/inverted file index存储.png)

这是采用inverted   file   index的存储形式，可以看到code单词存在于文档id为1和4的文档中，这样就可以得到包含code单词的文档了。

​	而在full  inverted  index中，采用的是这种形式，里面存储的是对儿（pair），包含着文档id和位置：

![full inverted index存储](../图片/数据库截图/full inverted index存储.png)

可以很直观的看出单词code存在于文档id为1的文档中的第6个单词。这样的好处是定位方便，坏处是占据的存储空间要更大。

#### InnoDB全文检索

​	InnoDB存储引擎从1.2版本开始支持全文检索，其采用full  inverted  index的计数。具体在InnoDB上是怎么实现全文检索的这里不再详述。

#### MySQL的全文检索

​	MySQL的全文检索有三种检索方式：

- Nature  Langusge，自然语言检索
- Boolean,布尔值检索
- Query  Expansion，扩展查询检索。

## MySQL中的锁

### 什么是锁

​	数据库系统使用锁是为了支持对共享资源进行并发访问，提供数据的一致性和完整性。在InnoDB存储引擎中，会在**行级别上对数据进行上锁**，当然，其也会在数据库中的其它位置进行上锁。

​	而对于MyISAM引擎，其锁是**表锁设计**。在并发情况下，读没有问题，而在并发插入的时候性能就要差一点了。

### lock与lacth

​	latch一般称为轻量级的锁，其要求锁定时间非常的短。在InnoDB存储引擎中，latch分为mutex（互斥锁）和rwlock（读写锁）。其目的是用来保证并发线程操作临界资源的正确性，并且通常没有死锁检测机制。

​	lock的对象是事务，用来锁定的是数据库中的对象，如表、页、行。并且一般lock的对象仅在事务提交或回滚之后才会释放。而在lock中，一般是有死锁机制的。

![lock与lacth的比较](../图片/数据库截图/lock与lacth的比较.png)

### InnoDB支持的锁

#### 共享锁和排它锁

​	InnoDB实现了标准的行级锁定，这是其一大优势；其行级锁分为两种类型：

- **共享锁（shared lock，S锁）：**允许持有锁的事务读取行记录；
- **排它锁（exclusive lock，X锁）：**允许持有锁的事务更新或删除一行记录。

InnoDB对这两种锁的规定是：

​	如果一个事务已经占有了该行记录的共享锁，那么另一个事务对于该行共享锁的请求将被立即授予，对于排它锁的授予将不被允许；

​	如果一个事务已经占有了该行的排它锁，那么另一个事务对该行无论何种锁的请求都不会被允许。

#### 意图锁（意向锁）

​	InnoDB支持多粒度的锁，即其**支持行锁与表锁共存**。在InnoDB中要想在多粒度上实现锁定，就可以使用意图锁。

​	意向锁是标记锁定，指示事务稍后将在该表中的行上获取什么样的锁（获取共享锁或者排它锁）。其支持的意向锁有两种：

- **意向共享锁（IS lock）：**代表事务想要获得该表中某几行的共享锁
- **意向排它锁（IX lock）：**代表事务想要获得表中某几行的排它锁

意向锁的协定如下：

- 在事务获取表中某几行的共享锁时，其必须首先获得在该表上的意向共享锁
- 在事务获取表中某几行的排它锁时，必须获得意向排它锁

其兼容性为：

![意向锁兼容性](../图片/数据库截图/意向锁兼容性.png)

可以从上表中看出：

- 意向锁之间实际上是没有冲突的。
- 意向锁与行锁之间才可能出现冲突。
- 排它锁（行锁）与任何锁都是冲突的，包括IS、S、IX；
- 共享锁只与共享锁与意向共享锁兼容。

如果请求事务与现有锁兼容，则授予锁，但如果它与现有锁冲突则不会。事务等待直到释放冲突的现有锁。如果锁定请求与现有锁冲突而无法被授予，则有可能造成死锁。

​	**意向锁是不会阻塞除了全表扫描之外的任何请求，所以，意向锁之间没有冲突的情况产生。意向锁的主要目的是为了显示某事物正在锁定行，或者说将要以何种锁锁定表中的某几行。**

#### 锁的算法

​	InnoDB有三种行锁的算法，分别如下：

##### 记录锁定

​	**记录锁定是对索引记录的锁定。**例如：

```SQL
SELECT c1 FROM t WHERE c1 = 10 FOR UPDATE;
```

在官方手册中，对于记录锁的描述是这样的：

```
A record lock is a lock on an index record. For example, SELECT c1 FROM t WHERE c1 = 10 FOR UPDATE; prevents any other transaction from inserting, updating, or deleting rows where the value of t.c1 is 10.

Record locks always lock index records, even if a table is defined with no indexes. For such cases, InnoDB creates a hidden clustered index and uses this index for record locking.
```

我对于这句话的理解是这样的：记录锁是对索引记录的锁，就像上面的那条SQL语句，采用了记录锁的方式，它的意义是阻止其他事物对该行记录的插入、更新、删除操作，这里的对该行记录代表对该行所对应的**索引记录**，**记录锁可以简单的理解为采用行锁方式。**

##### 间隙锁

​	**间隙锁是锁定索引之间的间隙，或者锁定在第一个索引之前的间隙或者最后一个索引的后面的间隙上。**

```
A gap lock is a lock on a gap between index records, or a lock on the gap before the first or after the last index record. For example, SELECT c1 FROM t WHERE c1 BETWEEN 10 and 20 FOR UPDATE; prevents other transactions from inserting a value of 15 into column t.c1, whether or not there was already any such value in the column, because the gaps between all existing values in the range are locked.
```

```SQL
-- 上述话中执行的语句
SELECT c1 FROM t WHERE c1 BETWEEN 10 and 20 FOR UPDATE
```

上面的一段话是说，该范围内的间隙都将被锁定，所以在在这个范围内进行的插入操作将会被阻塞。

使用唯一索引锁定行以搜索唯一行的语句不需要间隙锁定。（这不包括搜索条件仅包含多列唯一索引的某些列的情况;在这种情况下，确实会发生间隙锁定。）例如，如果`id`列具有唯一索引，则以下语句仅使用具有`id`值100 的行的索引记录锁定，其他会话是否在前一个间隙中插入行无关紧要：

```SQL
SELECT * FROM child WHERE id = 100;
```

如果`id`未编入索引或具有非唯一索引，则该语句会锁定前一个间隙。

##### next-key  lock

​	InnoDB采用next-key lock的策略来防止幻读的出现。因为该锁使得InnoDB不仅仅锁定查询涉及到的行，而且还会对索引中间的间隙进行锁定，以防止幻影行的插入。

#### 一致性非锁定读

​	**一致性非锁定读是指InnoDB采用多版本控制的方式来读取当前执行数据库中行的数据。**如果读取的行正在执行DELETE或者UPDATE等操作的话，这时读取操作并不会去等待行上锁的释放，相反的，InnoDB会去读取该行的一个快照数据。所谓的快照数据是指该行的之前版本的数据，而读取这个快照数据是不需要上锁的，因为这个快照数据是为了回滚用的，是一个历史数据。

​	非锁定读极大的提高了数据库的并发性。在InnoDB存储引擎的默认设置下，这是默认的读取方式，即读取不会占用和等待表上的锁。

​	快照数据其实就是当前行数据之前的历史版本。每行记录可能有多个版本，一行记录可能有不止一个快照数据，一般称这种技术为多版本技术。有多版本技术带来的并发控制称之为多版本并发控制（MVCC,Multi version concurrency  control）。

​	在事务隔离级别读已提交和可重复读下，InnoDB存储引擎默认采用非锁定的一致性读。然而，在这两种事务隔离级别下，对于快照数据，对于快照数据的定义是不同的：

- 在读已提交事务隔离级别下：

  非一致性读总是读取被锁定行的最新一份快照数据

- 在可重复读事务隔离级别下：

  非一致性读总是读取事务开始时的行数据版本。

下面看一个例子：

```SQL
BEGIN -- 开启事务
SELECT * FROM product WHERE id = 1
```

首先在数据库的一个客户端A中执行了上面的语句，开启了事务，并查询了id为1的记录，但是并没有提交事务；与此同时，再开启一个客户端B，执行如下语句：

```SQL
BEGIN
UPDATE product SET id=3 WHERE id=1
```

在客户端B中将id值为1记录的id值改为3，但是事务同样没有提交，这样在id=1的行其实加了一个锁。这时如果在客户端A中再次读取id为1的记录的话，根据InnoDB的特性，在读已提交或者可重复读的隔离级别下，会采用非锁定的一致性读取。

​	**这时在客户端A执行读取id为1的操作，两个隔离级别下的读取情况应该是一致的。都是读取到id为1,。而如果在客户端B提交了事务，由于在B中修改了一次，产生了一个快照版本，同样运行该语句在两个隔离级别下的读取情况就不同了。**

​	在读已提交的情况下：它总是读取行的最新版本，如果行被锁定了，则读取该行的一个最新的快照。由于B提交了事务，锁被释放了，则读取的时候则会读取到一个空值，因为id已经被改为3了。

​	在可重复读的情况下：会读取到事务开启时的状态，即还会读取到id为1的行记录。

![示例执行过程](../图片/数据库截图/示例执行过程.png)

#### 一致性锁定读

​	在InnoDB默认的配置下，数据库在可重复读的隔离级别下，InnoDB采用一致性非锁定读；但是在某些情况下，用户需要对数据库的读取操作进行显示的加锁以保证数据的逻辑一致性。而这要求数据库支持加锁语句，即使只对于SELECT语句。InnoDB存储引擎对于数据库支持两种锁定读取：

- SELECT ...FOR UPDATE;
- SELECT ... LOCK IN SHARE MODE;

第一条语句对读取操作加一个X锁，其它事务不能对已锁定的行加上任何锁。

第二条语句对读取的行记录加上一个S锁，其它事务可以对该行加S锁，但不能加X锁。

### MyISAM中的表锁

​	在前面已经知道，MyISAM支持的是表锁设计，其不支持行锁。所以，MyISAM在并发下，会对整张表进行加锁，读取时对读取到的所有表加共享锁，写入时对所有用到的表加排它锁。但是在表有读取查询的同时，也可以往表中插入新的记录（这被称为MyISAM的并发插入特性，CONCURRENT INSERT）。

### 数据库中的乐观锁与悲观锁

​	乐观锁与悲观锁都是人们定义出来的概念，是人们为了解决并发控制而产生的逻辑上的观念，所以，并不需要和数据库中的锁机制一一对应。

#### 乐观锁

​	在关系数据库管理系统里，乐观并发控制（又名“乐观锁”，Optimistic Concurrency Control，缩写“OCC”）是一种**并发控制的方法**。**它假设多用户并发的事务在处理时不会彼此互相影响，各事务能够在不产生锁的情况下处理各自影响的那部分数据。在提交数据更新之前，每个事务会先检查在该事务读取数据后，有没有其他事务又修改了该数据。如果其他事务有更新的话，正在提交的事务会进行回滚。**乐观事务控制最早是由孔祥重（H.T.Kung）教授提出。

​	乐观锁是相对于悲观锁的一种概念，它基于一个假设：乐观锁假设认为数据在一般情况下不会造成冲突，所以在数据进行更新提交时，才会正式对数据的冲突与否进行检测，如果发现冲突了，则让用户返回错误的信息，让用户去决定该该怎么解决这个冲突。

​	相对于悲观锁，乐观锁一般并不会使用数据库提供的锁机制，而是采用记录数据版本的方式来实现乐观锁。

​	实现乐观锁的方式有两种：

- **使用版本号**

  使用版本号时，可以在数据初始化时指定一个版本号，每次对数据的更新操作都对版本号执行+1操作。并判断当前版本号是不是该数据的最新的版本号。

```SQL
--1.查询出商品信息
select (status,status,version) from t_goods where id=#{id}
--2.根据商品信息生成订单
--3.修改商品status为2
update t_goods 
set status=2,version=version+1
where id=#{id} and version=#{version};
```

- **使用时间戳**：不详细叙述

#### 悲观锁

​	在关系数据库管理系统里，悲观并发控制（又名“悲观锁”，Pessimistic Concurrency Control，缩写“PCC”）是一种并发控制的方法。它可以阻止一个事务以影响其他用户的方式来修改数据。如果一个事务执行的操作都某行数据应用了锁，那只有当这个事务把锁释放，其他事务才能够执行与该锁冲突的操作。
​	悲观并发控制主要用于数据争用激烈的环境，以及发生并发冲突时使用锁保护数据的成本要低于回滚事务的成本的环境中。	

​	悲观锁，正如其名，它指的是对数据被外界（包括本系统当前的其他事务，以及来自外部系统的事务处理）修改持保守态度(悲观)，因此，在整个数据处理过程中，将数据处于锁定状态。 悲观锁的实现，往往依靠数据库提供的锁机制 （也只有数据库层提供的锁机制才能真正保证数据访问的排他性，否则，即使在本系统中实现了加锁机制，也无法保证外部系统不会修改数据）

​	**悲观锁的流程如下：**

- 在对任意记录进行修改前，先尝试为该记录加上排他锁（exclusive locking）。
- 如果加锁失败，说明该记录正在被修改，那么当前查询可能要等待或者抛出异常。 具体响应方式由开发者根据实际需要决定。
- 如果成功加锁，那么就可以对记录做修改，事务完成后就会解锁了。
- 其间如果有其他对该记录做修改或加排他锁的操作，都会等待我们解锁或直接抛出异常。

​       **MySQL InnoDB中使用悲观锁：**

首先得关闭数据库的自动提交属性。

```SQL
//0.开始事务
begin;/begin work;/start transaction; (三者选一就可以)
//1.查询出商品信息
select status from t_goods where id=1 for update;
//2.根据商品信息生成订单
insert into t_orders (id,goods_id) values (null,1);
//3.修改商品status为2
update t_goods set status=2;
//4.提交事务
commit;/commit work;
```

#### 乐观锁与悲观锁的区别

- 乐观锁是在提交数据的那一刻才会去判断是否产生了并发问题，而悲观锁是在数据操作还没开始之前就应该先考虑并发的可能。
- 乐观锁比较依靠用户的判断；悲观锁是依靠数据库的锁机制来严格执行的。
- 乐观锁适用于写入较少的情况；悲观锁适用于大量写入操作，或者读写频繁的系统中。