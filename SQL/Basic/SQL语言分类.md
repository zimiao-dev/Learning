# SQL 语言分类

SQL（Structured Query Language）是用于管理和操作**关系型数据库**的标准语言。

## DDL： Data Definition Language 数据定义语言

> 用于定义和管理数据库对象的结构

- **CREATE**：创建数据库对象

- **ALTER**：修改数据库对象结构

- **DROP**：删除数据库对象

- **TRUNCATE**：清空表数据，但保留结构

- **RENAME**：重命名数据库对象

```SQL

--- 创建表

CREATE TABLE IF NOT EXISTS Employees(id INT, name VARCHAR(20));

--- 修改表结构：添加字段

ALTER TABLE Employees ADD COLOUMN email VARCHAR(50);

--- 删除表

DROP TABLE Employees;

--- 清空表 数据

TRUNCATE TABLE Employees;

--- 重命名表名

RENAME TABLE Employees TO NEW_Employees;

```

> 操作对象是数据库对象，而不是数据本身

---

## DML：Data Manipulation Language 数据操作语言

> 对数据库表中数据进行增删操作

- INSERT：插入数据

- UPDATE：更新数据

- DELETE：删除数据

- INSERT ... ON DUPLICATE KEY UPDATE: 存在则更新，不存在则插入

```SQL

--- 插入数据 

INSERT INTO Employees (id, name) VALUES ('1', 'Alice');

--- 更新数据

UPDATE Employees SET name = 'Lily' WHERE id = 1;

--- 删除数据

DELETE FROM Employees WHERE id = 1;

--- 存在则更新，不再则插入

INSERT INTO 
    Employees (id, name)
VALUES 
    (1, 'Alice')
ON DUPLICATE KEY UPDATE
    name = 'Alice';

```

> 操作的是实际数据

> 通过WHERE语句可以精确筛选操作范围

---

## DQL：Data Query Language 数据查询语言

> 用于查询数据库中的数据

- SELECT: 查询数据

```SQL

--- 查询所有员工

SELECT * FROM Employees;

--- 条件查询

SELECT name FROM Employees WHERE id = 1;

```

---

## DCL: Data Control Language 数据控制语言

> 控制数据库的访问权限和事务处理，保障数据安全

- GRANT：授予权限

- REVOKE：撤销权限

- COMMIT：提交事务

- ROLLBACK：回滚事务

```SQL

--- 授予用户SELECT，INSERT 权限

GRANT SELECT, INSERT ON Employees TO 'user1'@'localhost';

--- 撤销 INSERT 权限

REVOKE INSERT ON Employees FROM 'user1'@'localhost';

--- 提交事务

COMMIT;

--- 回滚事务

ROLLBACK;

```

> 通常为 DBA DataBase Administrator 数据库管理员使用
> 管理用户权限和事务控制

