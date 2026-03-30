# Database Basic

## Structured Query Language (SQL)

| Short Name  |          Full Name           | Chinese Name | Notes                                    |
| :---------: | :--------------------------: | :----------: | :--------------------------------------- |
| [DDL](#data-definition-language-ddl) |   Data Definition Language   | 数据定义语言 | 操作数据库本身结构                       |
| [DML](#data-manipulation-language-dml) |  Data Manipulation Language  | 数据操作语言 | 操作表中数据行                           |
| [DQL](#data-query-language-dql) |     Data Query Language      | 数据查询语言 | 从一张或多张表中按照设定获取数据         |
| [DCL](#data-control-language-dcl) |    Data Control Language     | 数据控制语言 | 管理用户对数据库的访问权限，确保数据安全 |
| [TCL](#transaction-control-language-tcl) | Transaction Control Language | 事务控制语言 | 用于管理事务   |

## Basic SQL Grammar

### Basic concepts

|     Concept     | 中文  | Description                                |
| :-------------: | :---: | :----------------------------------------- |
|  **Relation**   | 关系  | Table (rows/columns)                       |
|  **Attribute**  | 元组  | Table column                               |
|   **Domain**    |  域   | The set of allowable values for attributes |
|    **Tuple**    | 属性  | Table row                                  |
|   **degree**    |  度   | The number of attributes                   |
| **Cardinality** | 基数  | The number of tuples                       |

| Relational Keys    | 中文     | Description                                                                             | 备注                             |
| ------------------ | -------- | --------------------------------------------------------------------------------------- | -------------------------------- |
| **Super key**      | 超键     | An attribute, or set of attributes, that uniquely identifies a tuple within a relation. | 一个键或多个键，可唯一标识每一行 |
| **Candidate Key**  | 候选键   | Minimal set of attributes identifying tuples                                            | 超键中最小的子集                 |
| **Primary Key**    | 主键     | The candidate key that is selected to identify tuples uniquely within the relation.     | 唯一，非空，不可更改             |
| **alternate keys** | 备用键   | The candidate keys that are not selected to be the primary key                          |                                  |
| **Foreign Key**    | 外键     | References primary key of another table                                                 | 表间关系                         |
| **composite key**  | 复合主键 | The primary key that consists of more than one attribute                                |                                  |

### Numerical Data Types

| Category       | Data Type         | Size/Range/Format                                                                             | Example                                                       |
| -------------- | ----------------- | --------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| Integer        | SMALLINT          | 2 Bytes<br>Range:-32,768 to 32,767                                                          |                                                               |
|                | INT/INTEGER       | 4 Bytes<br>Range:-2,147,483,648 to 2,147,483,647                                            |                                                               |
|                | BIGINT            | 8 Bytes                                                                                       |                                                               |
| Fixed-Point    | DECIMAL(M *, D*)  | M: total digits, D: decimal places                                                            | DECIMAL(5,2) : -999.99 to 999.99                              |
| Floating-Point | FLOAT(p)          | IEEE 754 standard <br /> p = precision bits (32-bit single or 64-bit double precision)        | FLOAT(24)                                                     |
| String         | CHAR(M)           | Fixed-length string (M: 0–255)<br />Right-padded with spaces, trimmed on retrieval.           |                                                               |
|                | VARCHAR(M)        | Variable-length string (M: 0–65,535)                                                          |                                                               |
| Date/Time      | DATE              | Format: YYYY-MM-DD<br />Range: 1000-01-01 to 9999-12-31                                       |                                                               |
|                | DATETIME *(fsp)*  | Format: YYYY-MM-DD hh\:mm\:ss.fraction<br />Range: 1000-01-01 00:00:00 to 9999-12-31 23:59:59 | YYYY-MM-DD hh\:mm\:ss.fff 3 decimal places, millisecond level |
|                | TIMESTAMP *(fsp)* | Stores UTC time, converts to client timezone on retrieval.                                    |                                                               |
|                | TIME              | Format: HH\:MM\:SS\[.微秒]                                                                    |                                                               |
|                | YEAR              | Format: YYYY                                                                                  |                                                               |

* *Italic* means optional parameters

### Common Symbols

| Symbol  | Name                         | 中文                 | Example                                                        | Discription                                                            |
| ------- | ---------------------------- | -------------------- | -------------------------------------------------------------- | ---------------------------------------------------------------------- |
| \`...\` | Escape identifier (backtick) | 转义标识符（反引号） | ``SELECT `order` FROM `table`;``                               | 避免标识符与 MySQL 保留关键字冲突 <br /> 允许标识符包含 特殊字符或空格 |
| '...'   | String                       | 字符串               | `WHERE name = 'John' `                                         | 包裹字符串内容                                                         |
| -- / #  | single-line comment          | 单行注释             | `-- comment `<br />`# also comment `                           |                                                                        |
| /\* \*/ | multiline comment            | 多行注释             | `/* Comment block */ `                                         |                                                                        |
| % / \_  | Wildcard (fuzzy matching)    | 通配符（模糊匹配）   | `LIKE 'a%' --匹配多个字符 `<br />` LIKE ‘j_hn’ --匹配单个字符` |                                                                        |

***

### Data Definition Language (DDL)

**Select Database**

```Mysql
USE schema_name     -- Select a database
```

**Create Table**

```Mysql
CREATE TABLE Persons (
                         id INT UNIQUE NOT NULL AUTO_INCREMENT,     -- 唯一，非空，自增
                         lastname VARCHAR(255) NOT NULL,            -- 非空
                         firstname VARCHAR(255),
                         age INT DEFAULT 12,                        -- 设置默认值
                         city VARCHAR(255)
) AUTO_INCREMENT = 5;                                               -- 设置自增初始值
```

**Column Options**

| Option          | Function                              | Example                            |
| --------------- | ------------------------------------- | ---------------------------------- |
| NOT NULL        | Force cannot be NULL                  | name VARCHAR(60) NOT NULL          |
| UNIQUE          | Must be unique                        | email VARCHAR(255) UNIQUE          |
| DEFAULT         | Set default value when no value given | age INT DEFAULT 18                 |
| AUTO\_INCREMENT | Increase value automatically          | id INT AUTO\_INCREMENT PRIMARY KEY |

**Modifying Tables**

`ALTER`

```Mysql
ALTER TABLE Students ADD COLUMN Address VARCHAR(255);               -- Add column

ALTER TABLE Students MODIFY Email VARCHAR(255);                     -- Modify column

ALTER TABLE Students DROP COLUMN Address;                           -- Drop column

ALTER TABLE Students CHANGE COLUMN Email EmailAddress VARCHAR(255); -- Rename column
```

### Data Manipulation Language (DML)

`INSERT` `UPDATE` `DELETE`

```Mysql
--Inserting Data
INSERT INTO Students (Name, Age, Gender)    -- 如果对应表的所有列，可以不写明列
VALUES ('Alice', 20, 'Female');

--Updating Data
UPDATE Students
SET Age = 21, Gender = 'Female'             -- 修改内容
WHERE StudentID = 1;                        -- 选择方式

--Deleting Data
DELETE FROM Students
WHERE StudentID = 1;
```

**Foreign Key**

Create and delete a foreign key:

```Mysql
CREATE TABLE orders (         -- 建表时定义
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    FOREIGN KEY (customer_id)
        REFERENCES customers(customer_id)
        [ON DELETE CASCADE
        ON UPDATE SET NULL]
);

ALTER TABLE orders            -- 修改表添加
ADD CONSTRAINT fk_customer
    FOREIGN KEY (customer_id)
    REFERENCES customers(customer_id)
    ON DELETE RESTRICT;

ALTER TABLE orders            -- 删除外键
DROP FOREIGN KEY fk_customer;
```

### 外键约束行为表

| 选项            | 删除操作行为描述                                       | 更新操作行为描述                       |
| --------------- | ------------------------------------------------------ | -------------------------------------- |
| **`CASCADE`**   | 删除父表记录时，**自动删除**子表关联记录               | 父表主键更新时，**自动更新**子表外键值 |
| **`SET NULL`**  | 删除父表记录时，子表外键设为**NULL**（字段需允许NULL） | 父表主键更新时，子表外键设为**NULL**   |
| **`RESTRICT`**  | **阻止**删除父表记录（默认行为）                       | **阻止**更新父表主键                   |
| **`NO ACTION`** | 同`RESTRICT`（标准SQL兼容）                            | 同`RESTRICT`（标准SQL兼容）            |

**注：与 `ON DELETE` 或 `ON UPDATE` 关联使用**

### Data Query Language (DQL)

Basic Queries

`SELECT`
```Mysql
SELECT [DISTINCT] [列名或表达式]
FROM [表名]
[WHERE 条件]            -- [IN / EXISTS]
[GROUP BY 分组列]
[HAVING 分组后条件]
[ORDER BY 排序列]       -- [ASC / DESC] 升序/降序
[LIMIT 返回行数];
```
`DISTINCT` 表示返回值中没有重复行
执行顺序：FROM → WHERE → GROUP BY → 聚合计算 → HAVING → SELECT → ORDER BY → LIMIT

##### Aggregate Functions 聚合函数
e.g. 使用聚合函数(数学函数)并创建新列
```Mysql
SELECT COUNT(*) AS TotalStudents, AVG(Age) AS AvgAge
FROM Students;
```

e.g. Grouping & Filtering

```Mysql
SELECT Department, AVG(Salary) AS AvgSalary
FROM Employees
GROUP BY Department
HAVING AVG(Salary) > 5000;
```

**Function 基本函数**
(Column) 若为算式 则先计算算式后sum(下同)

SUM() 求和函数

```Mysql
SELECT SUM(Column) [AS NewCloumn] FROM Table
[GROUP BY Column]                   -- 去除多余的重复列
[HAVING 条件];            -- 筛选求和后记录
```

自动忽略NULL

AVG() 平均值函数

```Mysql
SELECT AVG(Column) [AS NewCloumn] FROM Table;
```


COUNT() 计数函数

```Mysql
SELECT COUNT([DISTINCT] { * | expression })
FROM table
[WHERE conditions]
[GROUP BY columns];
```

expression:

* \* / 1 / constant :统计行数，不忽略NULL；
* column：统计非空值，忽略NULL；
* DISTINCT + 值： 统计唯一值；

MAX() MIN() 取最大值/最小值

```Mysql
SELECT
  MAX(column) AS max_value,
  MIN(column) AS min_value
FROM table_name
[WHERE conditions];
```

* 可对数值和时间/日期求最大最小值：
  * Max：最晚时间
  * Min：最早时间
* 默认字典集中按照ASCII码排布字符：'Z'>'a'

---
LIKE 比对通配符详解

*% - 匹配任意字符序列*

| 模式  | 说明       |
| ----- | ---------- |
| 'a%'  | 以a开头    |
| '%a'  | 以a结尾    |
| '%a%' | 包含a      |
| 'a%b' | a开头b结尾 |

*\_ - 匹配单个字符*

| 实例     | 说明          |
| -------- | ------------- |
| '\_at'   | 3字母以at结尾 |
| 'a\_\_'  | a开头的3字符  |
| '\_\_t%' | 第3位是t      |

转义字符：反斜杠 \\

---
#### Cross Product 交叉积
返回两个表的所有行组合
e.g.
```Mysql
SELECT first_name, name FROM Employees, Items;
```

#### Joins

| JOIN 类型              | 保留左表 | 保留右表 | 描述说明               |
| ---------------------- | -------- | -------- | ---------------------- |
| **INNER JOIN**         | ✘        | ✘        | 仅返回匹配行           |
| **LEFT JOIN**          | ✓        | ✘        | 保留左表所有行         |
| **RIGHT JOIN**         | ✘        | ✓        | 保留右表所有行         |
| **NATURAL JOIN**       | ✘        | ✘        | 自动按同名列进行内连接 |
| **NATURAL LEFT JOIN**  | ✓        | ✘        | 自动按同名列进行左连接 |
| **NATURAL RIGHT JOIN** | ✘        | ✓        | 自动按同名列进行右连接 |

e.g. Inner Join

```Mysql
SELECT S.Name, C.CourseName
FROM Students S
INNER JOIN Enrollments E ON S.StudentID = E.StudentID
INNER JOIN Courses C ON E.CourseID = C.CourseID;
```

e.g. Left Join

```Mysql
SELECT S.Name, C.CourseName
FROM Students S
LEFT JOIN Enrollments E ON S.StudentID = E.StudentID
LEFT JOIN Courses C ON E.CourseID = C.CourseID;
```

#### Set Operations

```Mysql
-- Union (distinct)
SELECT Name FROM ClassA
UNION
SELECT Name FROM ClassB;

-- Union All (includes duplicates)
SELECT Name FROM ClassA
UNION ALL
SELECT Name FROM ClassB;
```

#### Views 视图

e.g.Create a view
```Mysql
CREATE VIEW Employee_transaction_count AS
SELECT first_name, e_id, COUNT(t_id)
FROM Employees NATURAL JOIN Transactions
GROUP BY first_name, e_id;
```

---

#### Relational Algebra 关系代数

[Relational Algebra](./DatabaseTransactions.md#relational-algebra-关系代数)

##### Projection 投影
`SELECT FROM`$\pi$
```Mysql
SELECT * FROM Students;
```

##### Renaming 重命名
`AS` $\rho$
```Mysql
SELECT family_name AS surname FROM Employees;
```

##### Selection 选择
`SELECT FROM WHERE`$\sigma$
```Mysql
SELECT * FROM Employees WHERE first_name='Anne';
```
##### Cartesian Product 笛卡尔积
Cross Product 交叉积 $\times$

##### Natural Join 自然连接 $\Join$
---


## Data Control Language (DCL)

## Transaction Control Language (TCL)
[Transaction](./DatabaseTransactions.md)
* START TRANSACTION：开始事务。
* COMMIT：提交更改（持久化）。
* ROLLBACK：回滚所有操作（恢复原状）。
