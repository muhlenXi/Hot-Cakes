# SQL Cheat Sheet

## 0、基础知识

- 数据库 database
- 数据库管理系统 DBMS
- 列 column 行 row
- 主键 primary key

## 1、数据库操作

1-1、登录数据库

```shell
mysql -uroot -p
```

1-2、查询所有数据库

```sql
SHOW DATABASES;
```

1-3、创建数据库

```sql
CREATE DATABASE database_name;
CREATE DATABASE IF NOT EXISTS database_name;
```

1-4、删除数据库

```sql
DROP DATABASE database_name;
DROP DATABASE IF EXISTS database_name;
```

1-5、选择使用数据库

```sql
USE database_name;
```

1-6、清屏

```sql
SYSTEM CLEAR;
```

1-7、退出登录

```sql
EXIT;
QUIT;
```

1-8、查看当前数据库所有表

```sql
SHOW TABLES;
```

## 2、表操作

2-1、创建表

```sql
CREATE TABLE IF NOT EXISTS student
(
	id INT PRIMARY KEY,
	name VARCHAR(50) NOT NULL,
	age INT,
	add_time TIMESTAMP DEFAULT NOW(),
	math INT,
	english INT,
	school_id INT
);
```

2-2、复制已存在的表

```sql

-- 复制表
CREATE TABLE stu LIKE student;

-- 复制表和数据
CREATE TABLE student_copy AS SELECT * FROM student;
```

2-3、表内增加列

```sql
ALTER TABLE student ADD (gender varchar(10));
```

2-4、表内删除列

```sql
ALTER TABLE  student DROP gender;
```

2-5、重命名

```sql
-- 重命名表
ALTER TABLE old_table_name RENAME new_name;

-- 重命名列
ALTER TABLE school CHANGE id school_id INT;
ALTER TABLE school CHANGE name school_name VARCHAR(50);
```

2-6、删除表

```sql
DROP TABLE table_name;
```

2-7、清空表数据

```sql
TRUNCATE TABLE table_name;
```

2-8、添加/删除 约束

```sql
-- 主键约束
ALTER TABLE table_name ADD PRIMARY KEY (column);
ALTER TABLE table_name DROP PRIMARY KEY;

-- 主键自动增长
ALTER TABLE student MODIFY id INT AUTO_INCREMENT;
ALTER TABLE student MODIFY id INT;

-- 非空约束
ALTER TABLE school MODIFY school_name VARCHAR(50) NOT NULL;
ALTER TABLE school MODIFY school_name VARCHAR(50);

-- 唯一性约束
ALTER TABLE school MODIFY school_name VARCHAR(50) UNIQUE;
ALTER TABLE school DROP INDEX school_name;

-- 外键约束
ALTER TABLE student ADD CONSTRAINT student_school_fk 
	FOREIGN KEY (school_id) REFERENCES school(school_id)
	ON UPDATE CASCADE ON DELETE CASCADE;
	-- 级联更新 ON UPDATE CASCADE
	-- 级联删除 ON DELETE CASCADE

ALTER TABLE student DROP FOREIGN KEY student_school_fk;
```

2-9、获取表的描述信息

```sql
DESC table_name;
```

2-10、获取某一列的描述信息

```sql
-- DESCRIBE table_name column;
DESCRIBE student name;
```

##  3、表内记录操作

3-1、插入数据

```sql
INSERT INTO student (id, name) VALUES (3, "zhangsan"),(4, "lisi");

-- 插入查询数据
INSERT INTO student(id, name, gender, age, addTime, math, english, school_id)
SELECT id, name, gender, age, addTime, math, english, school_id FROM graduated_student;
```

3-2、删除数据

```sql
DELETE FROM table_name  WHERE condition;
```

3-3、修改数据

```sql
UPDATE student SET column_1 = value_1 WHERE condition;

UPDATE student set math = null WHERE id = 1;
UPDATE student SET math = 79, english = 60 WHERE id = 1;
```

3-4、查询数据

**SELECT 子句顺序： SELECT FROM WHERE GROUP BY HAVING ORDER BY**

```sql
-- 遍历
SELECT * FROM table_name;
SELECT column, column2 FROM table_name;
SELECT * FROM table_name WHERE condition;

-- 排序
SELECT * FROM table_name ORDER BY column ASC;
SELECT * FROM table_name ORDER BY column DESC;

-- 分页
SELECT column FROM table_name LIMIT number;
SELECT column FROM table_name LIMIT number OFFSET offset_number;

-- 筛选
SELECT name FROM student WHERE gender IS NULL;
SELECT name FROM student WHERE gender = "men" OR age = 28;
SELECT name FROM student WHERE age BETWEEN 10 AND 30;
SELECT name FROM student WHERE age IN (18, 28,30);
SELECT name FROM student WHERE NOT age = 18;
SELECT name FROM student WHERE name LIKE '孙%';

-- 计算字段
SELECT CONCAT(name,age) AS newName FROM student;
SELECT name, age, 100-age AS remainAge FROM student;
SELECT name FROM student WHERE DATE_FORMAT(addTime,'%Y') = 2019;
SELECT name,math,english,math+english FROM student;
SELECT name,math,english,math+IFNULL(english,0) AS total FROM student;
SELECT * FROM student  LIMIT 0,3;

-- 聚合函数
SELECT AVG(age) AS avg_age FROM student;
SELECT COUNT(*) AS num_student FROM student;
SELECT MAX(age) AS max_age FROM student;
SELECT MIN(age) AS min_age FROM student;
SELECT SUM(age) as sum_age FROM student;

-- 分组
SELECT gender, COUNT(*) AS number_gender FROM student GROUP BY gender;
SELECT gender, COUNT(*) AS number_gender FROM student GROUP BY gender HAVING COUNT(*) > 2;

-- 内联结
SELECT name, age, school_name FROM student, school WHERE student.school_id = school.school_id;
SELECT name, age, school_name FROM student INNER JOIN school ON student.school_id = school.school_id;

-- 自联结
SELECT id,name FROM student WHERE school_id = (SELECT school_id FROM student WHERE name = "张三");
SELECT s1.id, s1.name FROM student AS s1, student AS s2 WHERE s1.school_id = s2.school_id AND s2.name = '张三';

-- 组合
SELECT name, age, school_id FROM student WHERE school_id = 100
UNION
SELECT name, age, school_id FROM student WHERE school_id = 200;
```


3-5、添加注释

```sql
SELECT name FROM student;  -- 这是一条注释
```

## 4、表设计方法

| 表关系 | 实现方式 |
|:--:|:--:|
| 多对一 | 在多的一方建立外键，指向一的一方的主键。 |
| 多对多 | 需要借助中间表，中间表至少包含两列，这两列作为中间表的外键，分别指向两张表的主键。 |
| 一对一 | 在任意一张表添加唯一外键指向另一方的外键。 |

## 5、数据库权限管理

5-1、查询用户

```sql
USE mysql;
SELECT * FROM user;
```

5-2、添加用户/删除用户

```sql
-- 添加用户
-- CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
CREATE USER 'xiyinjun'@'localhost' IDENTIFIED BY '123456';

-- 删除用户
-- DROP USER ‘username’@‘localhost’;
DROP USER 'xiyinjun'@'localhost';
```

5-3、授权用户/取消用户授权

```sql
-- 授权用户
-- GRANT type_of_permission ON database_name.table_name TO ‘username’@'localhost’;
GRANT ALL PRIVILEGES ON *.* TO 'xiyinjun'@'localhost';

-- 取消授权用户
-- REVOKE type_of_permission ON database_name.table_name FROM ‘username’@‘localhost’;
REVOKE ALL PRIVILEGES ON *.* FROM 'xiyinjun'@'localhost';

-- 刷新授权
FLUSH PRIVILEGES;
```

## 6、数据库备份与还原

6-1、导出数据库

*直接在 Terminal 中操作*

```shell
# mysqldump -u 用户名 -p 数据库名 > 导出的地址/导出的文件名
mysqldump -uroot -p talent > '/Users/muhlenxi/Desktop/talent.sql'
```

6-2、导入数据库

*前提是需要登录上 MySQL 数据库*

```sql
CREATE DATABASE name;
USE name;
SOURCE /Users/muhlenxi/Desktop/talent.sql;
```

## 7、事务与隔离

事务的四大特性：

- Atomicity 原子性
- Consistency 一致性
- Isolation 隔离性
- Durability 持久性

```sql
-- 开启事务
START TRANSACTION;
UPDATE bank SET account=account-500 WHERE name='zhangsan';
UPDATE bank SET account=account+500 WHERE name='lisi';

-- 事务提交
COMMIT;

-- 事务回滚
ROLLBACK;

-- 查看事务提交方式
SELECT @@autocommit;

-- 修改事务提交方式 1-自动 0-手动
SET @@autocommit=0;
```

隔离原因: 

- 脏读
- 不可重复读
- 幻读

```sql
-- 查看事务隔离级别
SELECT @@transaction_isolation;

-- 设置事务隔离级别
-- RU
SET GLOBAL TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
-- RC
SET GLOBAL TRANSACTION ISOLATION LEVEL READ COMMITTED;
-- RR
SET GLOBAL TRANSACTION ISOLATION LEVEL REPEATABLE READ;
-- S
SET GLOBAL TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```
