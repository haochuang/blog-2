---
title: MySQL基础
date: 2017-12-22 01:05:59
---


## MySQL环境搭建

安装MySQL：`sudo apt install mysql-server mysql-client`

验证MySQL已启动：`sudo netstat -anp | grep mysql`

启动MySQL（三种均可）：  
```bash
sudo service mysql start
sudo /etc/init.d/mysql start
sudo systemctl start mysql.service
```

查看MySQL运行状态（三种均可）：  
```bash
service mysql status
/etc/init.d/mysql status
systemctl status mysql.service
```

<!-- more -->

## MySQL基础使用

### 数据库操作

查看数据库：`SHOW DATABASES;`

连接数据库：`USE 数据库名`

新建数据库：`CREATE DATABASE 数据库名;`

删除数据库：`DROP DATABASE 数据库名;`

加载文件中的数据：`SOURCE 文件名（一般是.sql后缀）`

退出：`QUIT` 或者 `EXIT`

### 表操作

查看表：`SHOW TABLES;`

新建数据表：  
```sql
CREATE TABLE 表的名字
(
  列名a  数据类型(数据长度),
  列名b  数据类型(数据长度),
  列名c  数据类型(数据长度)
);
```

查看表：`SELECT * FROM 表的名字;`

插入数据：`INSERT INTO 表的名字(列名a, 列名b, 列名c) VALUES(值1, 值2, 值3);`

重命名表（三种均可）：  
```sql
RENAME TABLE 原名 TO 新名;
ALTER TABLE 原名 RENAME [TO ]新名;
删除表    DROP TABLE 表的名字;
```

增加一列：

```sql
ALTER TABLE 表的名字 ADD [COLUMN ]列名字 数据类型 约束[ AFTER 列名字| FIRST];
```

默认增加在最后一列，AFTER指定在某列后插入，FIRST指定插入第一列

删除一列：`ALTER TABLE 表的名字 DROP [COLUMN ]列名字;`

修改一列：

```sql
ALTER TABLE 表的名字 CHANGE 原列名 新列名 数据类型 约束;
```

可以修改列名、数据类型或约束；数据类型不能省略，否则重命名失败；修改数据类型可能导致数据丢失。

```sql
ALTER TABLE 表的名字 MODIFY 列名字 新数据类型;
```

可以修改数据类型。

修改表中的值：`UPDATE 表的名字 SET 列1=值1,列2=值2 WHERE 条件;`

删除记录：`DELETE FROM 表的名字 WHERE 条件;`

### MySQL常用数据类型

|数据类型|	大小（字节）|	说明|
|---|---|---|
|`INT`	|4	|整数|
|`FLOAT`	|4	|单精度浮点数|
|`DOUBLE`	|8	|双精度浮点数|
|`ENUM`	|	|枚举，单选，如性别：enum('a','b')|
|`SET`	|	|集合，多选：set('1', '2', '3')|
|`DATE`	|3	|日期：YYYY-MM-DD|
|`TIME`	|3	|时间：HH:MM:SS|
|`YEAR`	|1	|年份值：YYYY|
|`CHAR`	|0~255	|定长字符串|
|`VARCHAR`	|0~255	|变长字符串|
|`TEXT`	|0~65536	|长文本数据|

`CHAR`和`VARCHAR`的区别：
例如存储"abc", CHAR(10)占10字节（包括7个空字符） VARCHAR(12)占4字节（增加一个额外字节来存储字符串本身的长度）


## SQL的约束

`PRIMARY KEY`  主键

`DEFAULT`  默认值

`UNIQUE`  唯一：一张表中指定一列的值不能有重复，即这一列每个值都是唯一的

`FOREIGN KEY`(两种均可)    `[CONSTRAINT 约束名 ]FOREIGN KEY (外键名) REFERENCES 表的名字(主键)`

`NOT NULL`  非空

## SELECT语句

```sql
SELECT 要查询的列名 FROM 表的名字 WHERE 限制条件;
```

要查询的列名为`*`代表查询所有列

`WHERE`限制条件可以有数学符号 `=`  `<`  `>`  `>=`  `<=`

限制条件之间可以有逻辑关系 `AND`  `OR`

`x>=25 AND x<=30`  等价于  `x BETWEEN 25 AND 30`

`IN`与`NOT IN`    用于筛选在或不在某个范围内的结果（类似Python）

`LIKE`  模糊搜索，使用通配符：

* `_`代表一个字符
* `%` 代表若干个字符

`ORDER BY 键名 ASC/DESC`  对键进行升序/降序排序

SQL内置函数计算：

| | | | | | |
|---|---|---|---|---|---|
|函数	|COUNT|	SUM	|AVG	|MAX	|MIN|
|作用	|计数|	求和|	平均值|	最大值	|最小值|

`AS`  给值重命名

## 子查询

类似于函数嵌套

【示例】

```sql
SELECT of_dpt,COUNT(proj_name) AS count_project FROM project
WHERE of_dpt IN
(SELECT in_dpt FROM employee WHERE name='Tom');
```

## 连接查询

`JOIN` 将两个表拼接， `ON` 规定连接规则

【示例】

```sql
SELECT id,name,people_num
FROM employee,department
WHERE employee.in_dpt = department.dpt_name
ORDER BY id;
```

等价于

```sql
SELECT id,name,people_num
FROM employee JOIN department
ON employee.in_dpt = department.dpt_name
ORDER BY id;
```

## 其他基本操作

### 索引

创建索引（两种均可）：

```sql
ALTER TABLE 表的名字 ADD INDEX 索引名(列名);
CREATE INDEX 索引名 ON 表名字(列名);
```

查看索引：`SHOW INDEX FROM 表的名字;`

### 视图

视图是一种虚拟存在的表，数据来自原来的表中。

创建视图：`CREATE VIEW 视图名(列a,列b,列c) AS SELECT 列1,列2,列3 FROM 表的名字;`

创建视图的语句后半句是一个SELECT查询语句，因此视图也可以建立在多张表上（使用子查询或连接查询）

### 导入导出数据

导入数据：`LOAD DATA INFILE '文件名' INTO TABLE 表的名字;`

导出数据：`SELECT 列1,列2 INTO OUTFILE '文件名' FROM 表的名字;`

### 备份与恢复

备份与导出的区别：导出的文件只是保存数据库中的数据；而备份，则是把数据库的结构，包括数据、约束、索引、视图等全部另存为一个文件。

使用mysqldump备份:

```bash
mysqldump -u 用户 -p 密码 数据库名>备份文件名
mysqldump -u 用户 -p 密码 数据库名 表的名字>备份文件名
```

备份文件名一般是.sql后缀

恢复：

使用source恢复     `source 文件名`

使用<恢复    运行命令：`mysql -u 用户 -p 密码 数据库名 < 文件名`

