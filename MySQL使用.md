## MySQL使用

2019.1.18  王道烩

### 1 安装

```bash
sudo apt-get install mysql-server
```

安装完成之后需要进行配置。`mysql_secure_installation` 然后按照提示配置相关信息。

**注意：**在启动mysql的时候必须要使用sudo，然后输入在上一步上的密码。

登录命令：

```bash
sudo mysql -u root -p
```

为了免`sudo`登录，可以使用下面的方法:

首先查找mysql数据库中的user表，找到user为root的plugin值，发现为auth_socket.

然后修改密码规则的配置：

```bash
set global validate_password_policy=0;
set global validate_password_mixed_case_count=0;
set global validate_password_number_count=3;
set global validate_password_special_char_count=0;
set global validate_password_length=3;
```

然后修改root密码：

`update mysql.user set authentication_string=PASSWORD('123'), plugin='mysql_native_password' where user='root';`

最后刷新：

`plush privileges;`

然后就可以免使用sudo来登录了。

图形化管理工具：MySQL_Workbench. 使用的端口默认是3306.

远程登录语句：

```bash
mysql -h 10.0.1.99 -u root -p
```



### 2 常用操作命令

| 命令                       | 功能               |
| -------------------------- | ------------------ |
| show databases;            | 显示数据库         |
| show tables;               | 显示当前数据库的表 |
| use database_name          | 使用某个数据库     |
| create database 数据库名称 | 创建数据库         |
| drop database 数据库名称   | 删除数据库         |
| drop table 表名            | 删除表             |
| DESC 表名                  | 显示表结构         |
| SHOW CREATE TABLE 表名     | 查看创建表的语句   |
|                            |                    |
|                            |                    |

### 3 基本查询语句

```sql
SELECT * FROM <表名>                                   
SELECT * FROM <表名> WHERE <条件表达式>
SELECT 列1, 列2, 列3 FROM  <表名> WHERE <条件表达式>

SELECT id, name, gender, score
FROM students
WHERE class_id = 1
ORDER BY score DESC;

SELECT AVG(score) average FROM students WHERE gender = 'M'; # average 表示这个新的列的名称

SELECT class_id, COUNT(*) num FROM students GROUP BY class_id;  # 分组进行查询

SELECT class_id, gender, COUNT(*) num FROM students GROUP BY class_id, gender; # 两个分组标准

SELECT
    s.id sid,
    s.name,
    s.gender,
    s.score,
    c.id cid,
    c.name cname
FROM students s, classes c;       # 多表查询，结果是多个表的笛卡尔积。


```

### 4 连接查询

```sql
SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
FROM students s
INNER JOIN classes c
ON s.class_id = c.id;   # 内连接
```

内连接的查询写法：

- 确定主表，使用FROM方法。
- 确定需要连接的表，使用INNER JOIN<表2>的方法。
- 确定链接条件：使用ON<条件> 
- 可选:加上WHERE ORDER By等子句。

其他连接方式：

- INNER JOIN 两张表都存在的记录
- LEFT OUTER JOIN 加上所有左表的记录(未匹配)
- RIGHT OUTER JOIN 加上所有右表的记录(未匹配)。
- FULL  OUTER JOIN 加上左右表的所有记录(未匹配)。



### 5 修改数据

```sql
INSERT INTO <表名> (字段1, 字段2, ...) VALUES (值1, 值2, ...);  添加一个记录。

INSERT INTO students (class_id, name, gender, score) VALUES (2, '大牛', 'M', 80);

INSERT INTO students (class_id, name, gender, score) VALUES
  (1, '大宝', 'M', 87),
  (2, '二宝', 'M', 81);                          # 一次添加多条记录。
  
  
UPDATE <表名> SET 字段1=值1, 字段2=值2, ... WHERE ...;   #更新记录。
UPDATE students SET name='大牛', score=66 WHERE id=1;

DELETE FROM <表名> WHERE ...;   删除一个记录。

DELETE FROM students WHERE id>=5 AND id<=7;

```





