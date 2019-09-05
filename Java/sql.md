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
```

1-8、查看当前数据库所有表

```sql
SHOW TABLES;
```

## 2、表操作

2-1、创建表

```sql
CREATE TABLE IF NOT EXISTS student(id int);
```

2-2、复制已存在的表

```sql
CREATE TABLE stu LIKE student;
```

2-3、表内增加列

```sql
ALTER TABLE student ADD (gender varchar(10));
```

2-4、表内删除列

```sql
ALTER TABLE  student DROP gender;
```

2-5、删除表

```sql
DROP TABLE table_name;
```

2-6、清空表数据

```sql
TRUNCATE TABLE table_name;
```

2-7、添加主键

```sql
ALTER TABLE table_name ADD PRIMARY KEY (column);
```

2-8、删除主键

```sql
ALTER TABLE table_name DROP PRIMARY KEY;
```

2-9、获取某一列的描述信息

```sql
DESCRIBE table_name column;
```

##  3、表内记录操作

3-1、增加数据

```sql
INSERT INTO student (id, name) VALUES (1, "zhangsan");
```

3-2、删除数据

```sql
DELETE FROM table_name  WHERE condition;
```

3-3、修改数据

```sql
UPDATE student SET column_1 = value_1 WHERE condition;
```

3-4、查询数据

**SELECT 子句顺序： SELECT FROM WHERE GROUP BY HAVING ORDER BY**

```sql
SELECT * FROM table_name;
SELECT column, column2 FROM table_name;
SELECT * FROM table_name WHERE condition;
SELECT * FROM table_name ORDER BY column ASC;
SELECT * FROM table_name ORDER BY column DESC;
SELECT column FROM table_name LIMIT number;
SELECT column FROM table_name LIMIT number OFFSET offset_number;

SELECT name FROM student WHERE gender IS NULL;
SELECT name FROM student WHERE gender = "men" OR age = 28;
SELECT name FROM student WHERE age BETWEEN 10 AND 30;
SELECT name FROM student WHERE age IN (18, 28,30);
SELECT name FROM student WHERE NOT age = 18;
SELECT name FROM student WHERE name LIKE '孙%';
SELECT CONCAT(name,age) AS newName FROM student;
SELECT name, age, 100-age AS remainAge FROM student;
SELECT name FROM student WHERE DATE_FORMAT(addTime,'%Y') = 2019;
SELECT name,math,english,math+english FROM student;
SELECT name,math,english,math+IFNULL(english,0) AS total FROM student;

SELECT AVG(age) AS avg_age FROM student;
SELECT COUNT(*) AS num_student FROM student;
SELECT MAX(age) AS max_age FROM student;
SELECT MIN(age) AS min_age FROM student;
SELECT SUM(age) as sum_age FROM student;

SELECT gender, COUNT(*) AS number_gender FROM student GROUP BY gender;
SELECT gender, COUNT(*) AS number_gender FROM student GROUP BY gender HAVING COUNT(*) > 2;
```


3-5、添加注释

```sql
SELECT name FROM student;  -- 这是一条注释
```


