## MySQL数据库操作
```sql
#创建仓库
CREATE DATABASE 数据库名称;

#查看
SHOW DATABASES;

#选择
USE 数据库名称;

#删除库
DROP DATABASE 数据库名称;
```
## MySQL数据表操作
```sql
# 创建表
create table table_name 
    (
        列名1 属性,
        列名2 属性,
        ...
    
    );
# 例
create table db_admin(
	id int auto_increment primarykey,
	user varchat(32) not null,
	password varchar(32) not null,
	createtime datetime
);
# 原始
CREATE [TEMPORARY] TABLE [IF NOT EXISTS] 数据表名[(create_definition,...)] [table_options] [select_statement]

# 查看数据表结构SHOW COLUMNS或DESCRIBE

SHOW [FULL] COLUMNS FROM 数据表名 [FROM 数据库名];
--或者
SHOW [FULL] COLUMNS FROM 数据库名.数据表名;

# 修改表结构ALTER TABLE

ALTER[IGNORE] TABLE 数据表名 alter_spec[,alter_spec]...
# 例
alter table tb_admin add email varchat(50) not null,modify user varchar(40);

# 重命名表RENAME TABLE

RENAME TABLE 数据表名1 To 数据表名2

# 删除表DROP TABLE
DROP TABLE 数据表名;
--或
DROP TABLE IF EXISTS 数据表名;

```
## 数据表高级操作
```sql
# 克隆表
--实现克隆表
--1.方法一 LIKE
CREATE TABLE 新表名 LIKE 被克隆表名;
INSERT INTO 新表名 SELECT * FROM 被克隆表名;

--2.方法二
CREATE TABLE 新表名 (SELECT * FROM 被克隆表名);

# 注意 复制会有瓶颈，不能完全复制，Key的属性就无法复制


#清空表
--清空表
--1.记录自增ID未删除
DELETE FORM 表名;
--DELETE是一行一行的删除记录。若有自增字段，再次添加会从原记录最大的自增字段开始写入记录

--2.记录自增ID删除
TRUNCATE TABLE 表名;
--TRUNCATE不会返回被删除条目，TRUNCATE是将表重建，速度快于DELETE，自增值从初始值开始

#创建临时表
CREATE TEMPORARY TABLE 表名(...);
--示例:
create temporary table tb_admin(
	id int(4) zerofill primary key auto_increment,
    name varchar(32) not null,
    sex char(2) not null
);

```
## MySQL语句操作
```sql
# 插入记录INSERT
insert into 数据表名[(column_name1,column_name2,...)] value(v1,v2,...)[,(v1,v2,...)];
--后面给定多个括号进行批量添加，中间使用逗号隔开

# 查询数据库记录SELECT
SELECT [DISTINCT] [CONCAT (col1,":",col2) as col] selection_list --要查询的内容,选择哪些列
FROM 数据表名tb_list											--指定数据表
WHERE primary_constraint									--查询时需要满足的内容
GROUP BY grouping_columns									--如何对结果进行分组
ORDER BY sorting_columns									--如何对结果进行排序
HAVING secondary_constraint									--查询时满足的第二条件
LIMIT count													--限定输出的查询结果

#删除记录DELETE
DELETE FROM 数据表名 WHERE condition;
```

## 数据库用户操作
```sql
# 创建用户

CREATE USER '用户名'@'来源地址' [IDENTIFIED BY [PASSWORD] '密码'];

-----------------------------------解释--------------------------------
'用户名'：指定将创建的用户名
'来源地址'：指定新创建的用户可在哪些主机上登录，可使用IP地址、网段、主机名的形式，
          本地用户可用localhost，允许任意主机登录可用通配符%
'密码'：若使用明文密码，直接输入'密码'，插入到数据库时由Mysql自动加密；
       若使用加密密码，需要先使用SELECT PASSWORD('密码'); 获取密文，再在语句中添加 PASSWORD '密文';
       若省略“IDENTIFIED BY”部分，则用户的密码将为空（不建议使用）
----------------------------------------------------------------------
例如：
create user 'test1'@'localhost' IDENTIFIED BY '123456';

select password('123456');
create user 'test2'@'localhost' IDENTIFIED
BY PASSWORD '*6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9';

```

```sql
查看用户信息
--切换数据库
use mysql;
select user,authentication_string,Host from user;
```

```sql
#重命名用户名
RENAME USER '用户名'@'来源地址' TO '新用户名'@'新来源地址';

```

```sql
# 删除用户
DROP USER '用户名'@'来源地址';

```
```sql
# 修改密码
--1.修改当前登陆用户密码
SET PASSWORD = PASSWORD('123');

--2.修改其他用户密码
SET PASSWORD FOR '用户名'@'来源地址' = PASSWORD('123');
```