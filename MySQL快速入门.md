[toc]

# 基本书写规则

1. ==所有情况都应该是英文半角输入==
2. 每句操作语句后面需要加上";"
3. win系统下不区分大小写，但是一般**关键字**为大写、**数据库、数据表、列**为小写



# 注释

> 1. -- 注释内容
> 2. #注释内容
> 3. /\*注释内容*/



# 数据库



## 查看

### 所有

>用法：SHOW DATABASES

```sql
SHOW DATABASES
```



### 条件

>用法：SHOW DATABASES LIKE '条件语句'

> 条件语句：
>
> 1. demo：表示与名称demo完全相同的数据库
> 2. %demo%：表示名称中含有demo的所有数据库
> 3. %demo：表示以demo结尾的数据库
> 4. demo%：表示以demo开头的数据库
>
> 
>
> ==% 为占位符==

```sql
SHOW DATABASES LIKE 'demo' -- '%demo%'
```



## 创建

> 用法：CREATE DATABASE 数据库名称
>
> CREATE DATABASE demo

```sql
CREATE DATABASE demo
```



### 进阶

> 用法：
>
> CREATE DATABASE IF NOT EXISTS 数据库名称
>
> DEFAULT CHARACTER SET 编码规则（如utf8）
>
> DEFAULT COLLATE 校对规则
>
> **注：**
>
> **IF NOT EXISTS **是防止已经存在该数据库时报错

```sql
/*
CREATE DATABASE demo
*/
CREATE DATABASE IF NOT EXISTS demo
DEFAULT CHARACTER SET utf8
DEFAULT COLLATE utf8_chinese_ci#中文简体
```



## 使用（进入）

> 用法：USE 数据库名称

```sql
USE demo
```



## 修改

> 用法：
>
> ALTER DATABASE 数据库名称
>
> DEFAULT CHARACTER SET 编码规则（如utf-8）
>
> DEFAULT COLLATE 校对规则
>
> *即除了关键词alter不一样外，其他都与创建数据库相同*

```sql
ALTER DATABASE demo
DEFAULT CHARACTER SET utf8
DEFAULT COLLATE utf8_chinese_ci
```



## 删除

> DROP DATABASE IF EXISTS 数据库名称
>
> **注：**
>
> **IF EXISTS**是防止不存在该数据库时报错

```sql
ROP DATABASE [IF EXISTS] demo
```

**注：**==不要轻易删除数据库，特别是安装后自带的数据库（如 information_schema 和 mysql ），删除后MySQL可能将会无法正常工作==





# 数据表

## 查看

> 用法：
>
> 1. SHOW TABLES
> 2. SHOW TABLES LIKE '条件语句'
> 3. DESCRIBE 名称

```sql
SHOW TABLES LIKE 'table1'
DESCRIBE table1
```



## 创建

> 用法：CREATE TABLE 数据表名称([定义选项])
>
> 定义选项：数据名称 数据类型 是否为NULL 附加说明

```sql
CREATE TABLE table1(id INT NOT NULL)
```



## 插入数据

> 用法：INSERT INTO 表名称 (para1,……,paran) VALUES ("","")

```sql
INSERT INTO 表名称 (url,content) VALUES ("www.csdn.net","csdn")
```



## 提取数据

> SELECT 字段 FROM 表名 WHERE 条件语句
>
> **注：**字段即需要显示的列，*为显示所有，亦可单独给出列名

```sql
SELECT * FROM urls WHERE id=1
SELECT id,url FROM urls WHERE id=1
SELECT id,url FROM urls WHERE url LIKE "%baidu%"
```



## 修改

1. 

> ALTER TABLE 表名 修改选项

> 修改选项：
>
> 1. 增加列：ADD COLUMN 列名 类型
> 2. 改变列属性：CHANGE COLUMN 旧列名 新列名 新列类型、MODIFY COLUMN 列名 类型
> 3. 删除列：DROP COLUMN 列名
> 4. 修改表名：RENAME TO 新表名
> 5. 字符集修改：CHARACTER SET 字符集名
> 6. 校对规则修改：COLLATE 校对规则名

```sql
#1.增加列
ALTER TABLE ADD COLUMN new "a"
#3.删除列
ALTER TABLE urls DROP COLUMN new
#4.表名修改
ALTER TABLE urls RENAME [TO] new
#5.字符集修改
ALTER TABLE urls CHARACTER SET gb2312  
#6.校对规则修改
ALTER TABLE urls COLLATE gb2312_chinese_ci
```

2. 

> UPDATE 表名 SET 修改内容 WHERE 条件语句

```sql
UPDATE urls SET url="google",content="谷歌" WHERE id=1
```



## 删除

1. 

> DELETE FROM 表名 WHERE 条件语句
>
> **注：**如果没有where语句，就会删除整张表

```sql
DELETE FROM urls WHERE id=1
```

2. 

> DROP TABLE IF EXISTS 表名1,…… 

```sql
DROP TABLE IF EXISTS urls
```





# 数据

## 数据类型

#### 二进制

| 名称          | 字节         |
| ------------- | ------------ |
| BIT(M)        | (M+7)/8      |
| BINARY(M)     | M            |
| VARBINARY(M)  | M+1          |
| TINYBLOB(M)   | L+1，L<2^8^  |
| BLOB(M)       | L+2，L<2^16^ |
| MEDIUMBLOB(M) | L+3，L<2^24^ |
| LONGBLOB(M)   | L+4，L<2^32^ |



#### 整数

| 名称      | 字节 |
| --------- | ---- |
| TINYINT   | 1    |
| SMALLINT  | 2    |
| MEDIUMINT | 3    |
| INT       | 4    |
| BIGINT    | 8    |



#### 浮点数

| 名称             | 字节 |
| ---------------- | ---- |
| FLOAT            | 4    |
| DOUBLE           | 8    |
| DECIMAL(M,D)/DEC | M+2  |

**注：**DECIMAL(M,D)中：**M**为精度，即总位数，**D**为标度，即小数位数。默认M=10，D=0



#### 字符串

| 名称       | 字节                   |
| ---------- | ---------------------- |
| CHAR(M)    | M，1<=M<=255           |
| VARCHAR(M) | L+1，L< = M且1<=M<=255 |
| TINYTEXT   | L+1，L<2^8^            |
| TEXT       | L+2，L<2^16^           |
| MEDIUMTEXT | L+3，L<2^24^           |
| LONGTEXT   | L+4，L<2^32^           |
| ENUM       | 取决于枚举值数量       |
| SET        | 取决于集合数量         |



#### 时间

| 名称      | 格式                | 范围                                              | 字节 |
| --------- | ------------------- | ------------------------------------------------- | ---- |
| YEAR      | YYYY                | 1901 ~ 2155                                       | 1    |
| TIME      | HH:MM:SS            | -838:59:59 ~ 838:59:59                            | 3    |
| DATE      | YYYY-MM-DD          | 1000-01-01 ~ 9999-12-3                            | 3    |
| DATETIME  | YYYY-MM-DD HH:MM:SS | 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59         | 8    |
| TIMESTAMP | YYYY-MM-DD HH:MM:SS | 1980-01-01 00:00:01 UTC ~ 2040-01-19 03:14:07 UTC | 4    |

**注：**

1. 非法值默认为0
2. 虽然在很多情况下不用上面的格式也是可行的，但最好还是形成统一的规范



## 转义字符

| 字符 | 转义后  |
| ---- | ------- |
| \\"  | "       |
| \n   | 换行    |
| \r   | 回车    |
| \t   | 制表符  |
| \0   | ASCII 0 |
| \b   | 退格符  |

**注：**区分大小写



## 变量

### 全局变量

#### 查看

> 用法：SHOW GLOBAL VARIABLES

```sql
SHOW GLOBAL VARIABLES
```

#### 设置

> 用法：
>
> 1. SET @@global.变量名=值
> 2. SET global 变量名=值
>
> **注：**更改全局变量，必须具有 SUPER 权限

```sql
SET @@global.g=default
SET global g=default
```



### 会话变量

#### 查看

> 用法：SHOW SESSION VARIABLES

```sql
SHOW SESSION VARIABLES
```

#### 设置

> 用法：
>
> 1. SET @@session.变量名=值
>
> 2. SET @@变量名=值
> 3. SET session 变量名=值
> 4. SET 变量名=值
>
> **注：**使用@@时首先标记会话变量，如果会话变量不存在，则标记全局变量

```sql
SET @@session.id=5;
SET session id=5;
SET @@id=5;
SET id = 5;
```







# 储存引擎

### 查看

>用法：SHOW ENGINES



### 作用

| 引擎       | 作用                                                         |
| ---------- | ------------------------------------------------------------ |
| ARCHIVE    | 用于数据存档的引擎，数据被插入后就不能在修改了，且不支持索引。 |
| CSV        | 在存储数据时，会以逗号作为数据项之间的分隔符。               |
| BLACKHOLE  | 会丢弃写操作，该操作会返回空内容。                           |
| FEDERATED  | 将数据存储在远程数据库中，用来访问远程表的存储引擎。         |
| InnoDB     | 具备外键支持功能的事务处理引擎                               |
| MERGE      | 用来管理由多个 MyISAM 表构成的表集合                         |
| MEMORY     | 置于内存的表                                                 |
| MRG_MyISAM | 主要的非事务处理存储引擎                                     |
| NDB        | MySQL 集群专用存储引擎                                       |



### 修改默认引擎

>用法：SET default_storage_engine=<名称>

```sql
SET default_storage_engine=<CSV>
```

