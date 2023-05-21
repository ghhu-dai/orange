# sql语句的执行顺序 

```sqlite
FROM
WHERE
GROUP BY 
HAVING
SELECT
DISTINCT
ORDER BY
```



# SQL基础语言学习

## CREATER TABLE - 创建表

```sqlite
CREATE TABLE 表名称
(
	列名称1 数据类型，
    列名称2 数据类型，
    列名称3 数据类型，
    ...
);
```

| 数据类型                                               | 描述                                                         |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| `integer(size),int(size),smallint(size),tinyint(size)` | 仅容纳整数，在括号内规定数字 的最大位数                      |
| `decimal(size d),numeric(size,d)`                      | 容纳带有小数的数字 ，size规定数字 的最大位数，d规定小数点右侧的最大位数 |
| `char(size)`                                           | 容纳 固定长度的字符串                                        |
| `varchar(size)`                                        | 容纳可变长度的字符串                                         |
| `data(yyyymmdd)`                                       | 容纳 日期                                                    |

例 ：创建Persons的表

```sql
CREATE TABLE Persons
(
Id_P int,
LastName varchar(255),
FirstName varchar(255),
Address varchar(255),
City varchar(255)
);
```







## INSERT 插入数据 

```sqlite
INSERT INTO table_name VALUES (值1，值2，...)； # 向表格中插入新的行
INSERT INTO table_name (列1，列2，...) VALUES (值1，值2...); # 无给定值将是null
```

例：

```sql
INSERT INTO Persons VALUES (1,'shoujun','dai','hubu','hubu');
INSERT INTO persons (Lastname,Address) VALUES ('hu','hubu');
```





## SELECT 查询数据 

```sqlite
SELECT * FROM table_name;  	# *表示 选取所有列
SELECT 列名称 FROM table_name;

```



## DISTINCT 去除重复值 

```sqlite
SELEcT DISTINCT column_name FROM table_name;
```







## WHERE 条件过滤

```sqlite
SELECT column_name FROM table_name 列 运算符 值 
```

| 操作符  | 描述         |
| ------- | ------------ |
| =       |              |
| <>      | 不等于       |
| >       |              |
| <       |              |
| >=      |              |
| <=      |              |
| BETWEEN | 在范围之内   |
| LIKE    | 搜索某种模式 |

`note`：在某些SQL版本中，操作符<>可以写作 !=



##  AND & OR 运算符

AND和OR 可在WHERE子语句中把两个或多个条件结合起来

```sqlite
SELECT * FROM table_name 列 运算符 值  AND/OR 列 运算符 值  # AND/OR 前后的式子要完整
```





## ORDER BY 排序 

```sqlite
SELECT * FROM table_name ORDER BY 列1，列2 DESC;  # DESC 降序，不指定默认升序ASC
```

`note`：在第一列有相同值(或NULL)时，第二列是以升序排序的，



## UPDATE 更新数据 

UPDATE语句用于修改表中的数据 

```sqlite
UPDATE table_name SET column_name = new_value WHERE 列名称 = 某值 ;
```





## DELETE删除数据 

```sqlite
DELETE FROM table_name WHERE column_name = value;

DELETE FROM table_name; // 在不删除表的情况下删除所有行，表的结构 属性 索引 都是完整的
```





## TRUNCATE TABLE 清除表数据 

```sqlite
TRUNCATE TABLE table_name; // 仅删除表的数据
```



## DROP TABLE 删除表

```sqlite
DROP TABLE table_name; // 表的结构 属性 索引 也会被删除
```







# SQL高级语言学习



## LIKE 查找类似值 

```sqlite'
SELETE 列名/* FROM 表名称 WHERE 列名称 LIKE 值 ；

点位符：
% : 表示所有内容，如 'N%'表示 以N开头的，‘%g’表示以g结尾的，‘%on%’表示包含on的

// 和 NOT 搭配使用 NOT LIKE
```



## IN 锁定多个值 

IN操作符允许我们在WHERE子句中规定多个值 

```sqlite
SELETE 列名/* FROM 表名称 WHERE 列名称 IN (值1，值 2，值 3)；
```





## BETWEEN 选取区间数据 

BETWEEN...AND 会选取介于两个值之间的数据范围，***[ ),左闭右开区间*,不同数据库操作不同**

```sqlite
SELECT 列名/* FROM 表名称 WHERE 列名称 BETWEEN 值1 AND 值 2；

与NOT搭配
NOT BETWEEN ... AND ...
```



## AS 别名

```sqlite
SELECT 列名称/* FROM 表名称 AS 别名 ； // 为表创建别名
SELECT 列名称 AS 别名 FROM 表名称； // 为列创建别名

// AS 可以省略，但列别名需要加上”“
```





## JOIN 多表关联

多表关联的前提是***主键***

​	主键是一个列，这个 列中的第一行的值 都是唯一的，以不重复每个表中的所有数据 的情况下，把表间的数据交	叉捆绑在一起

```sqlite
SELECT 列名 FROM 表A JOIN_type 表B ON 表A主键列 = 表B主键列；
```

JOIN的类型

|                   |                                            |
| ----------------- | ------------------------------------------ |
| JOIN / INNER JOIN | 内部连接，返回两表中匹配的行               |
| LEFT JOIN         | 左连接，从左表中返回所有的行               |
| RIGHT JOIN        |                                            |
| FRLL  JOIN        | 全连接，只有其中一个表中存在匹配，就返回行 |





## UNION 合并结果集

`UNION` 操作符用于合并两个或多个SELECT语句的结果集

```sqlite
SELECT 列名 FROM 表A
UNION
SELECT 列名 FROM 表B；

// union操作符默认选取不同的值 ，如果查询结果需要显示重复的值，使用UNION ALL
```





## NOT NULL 非空

`NOT NULL`强制约束列不接受NULL值 

```sqlite
CREATE TABLE 表名
(
    列 int NOT NULL
)
```





## VIEW 视图

```sqlite
CREATE OR REPLACE VIEW 视图名 AS
SELECT 列名
FROM 表名
WHERE 查询条件；

// 删除视图
DROP VIEW 视图名；
```





# SQL 常用函数学习



## AVG 平均值 

返回数值列的平均值 ，不包括NULL

```sqlite
SELECT AVG(列名) FROM 表名；
```



## COUNT 汇总行数

返回匹配指定条件的行数，不包括NULL

```sqlite
SELECT COUNT(*) FROM 表名； 			// 返回表中的记录数
SELECT COUNT(DISTINCT 列名) FROM 表名； // 返回指定列的不同值的数目
SELECT COUNT(列名) FRONM 表名； 			// 返回指定列的值的数目
```



## MAX 最大值 

返回一列的最大值 ，null不包括在计算中

如果 用于文本列，获得按字母顺序排列的最高或最低值 

```sqlte
SELECT MAX(列名) FROM 表名；
```



## MIN 最小值 

返回一列中的最小值 ，null不包括在计算中

```sqlite
SELECT MIN(列名) FROM 表名；
```





## SUM 求和

返回数值列的总数

```sqilte
SELECT SUM(列名) FROM 表名；
```



## GROUP BY 分组

用于结合合计函数 ，根据一个或多个列 对结果集进行分组

```sqlite
SELECT 列名A，统计函数（列名B）
FROM 表名
WHERE 查询条件
GROUP BY 列名A；
```



## HAVING 句尾连接

在SQL中增加HAVING子句的原因是，WHERE关键字无法与合计函数一起使用

HAVING不可用于分组之前 的数据 

WHERE 不可用过分组之后 的数据 

```sqlite
SELECT 列名A 统计函数（列名B）
FROM 表名
WHERE 没有函数的查询条件
GROUP BY 列名A
HAVING 包含统计函数的查询条件；
```



## UCASE/UPPER 大写

把字段 的值 转换为大写

```sqlite
SELECT UPPER(列名) FROM 表名；

小写是LCASE/LOWER
```





## LEN/LENGTH 获取长度

返回文本字段中值 的长度

```sqlite
SELECT LENGTH(列名) FROM 表名；

// 如：
SELECT LENGTH(lastname),lastname FROM persons;
```



## ROUND 数值取舍

```sqlite
SELECT ROUND(列名，精度 ) FROM 表名；
// ROUND是四舍五入的
```





## NOW/SYSDATE 当前 时间

```sqlite
SELECT SYSDATE FROM 表名；
```

