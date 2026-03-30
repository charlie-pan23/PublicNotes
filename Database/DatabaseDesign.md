# Design Database

## ER Modeling

### Entity Relationships (ER)

*   Entity: A real world object that has attributes and relationships to other entities.
*   Attribute: A property of an entity.
*   Relationship: A link between two entities.

### Entity Relationship Diagram (ERD)

*   Cardinality Ratios
    *   One to one
        *   (1:1)
        *   *eg.* Each lecturer has a unique office & offices are single occupancy
    *   One to many
        *   (1\:M) or (1:\*)
        *   *eg.* A lecturer may tutor many students, but each student has just one tutor
    *   Many to many
        *   (M\:M) or (M\:N) or (\* : \*)
        *   *eg.* Each student takes several modules, and each module is taken by several
            students

*   Entities
    *   boxes with rounded corners 圆角矩形
    *   labelled with the name of the class of objects represented by that entity

*   Attributes
    *   ovals 椭圆
    *   Each attribute is linked to its entity by a line
    *   The name of the attribute is written in the oval

*   Relationships
    *   a diamond box 菱形框
    *   Commonly has two degrees
    *   The ends of the link show cardinality, use semicircle as many

### Designing ER Models

**identify from the requirements:**

*   Entities
*   Attributes
*   Relationships and Cardinality ratios

### From ER Diagram to SQL Tables

*   Entity → Table
*   Attribute → Column
*   Relationship → Foreign Key

## Normalisation (Optimize database)

**Normalisation is a technique of re-organising data into multiple related tables, so that data redundancy is minimised.**

### Functional Dependency (FD)

**If A and B are attribute sets of relation R, B is functionally dependent on A (denoted A → B), if each value of A in R
is associated with exactly one value of B in R.**

*   A : determinant 决定因素
*   B : dependent 因变量
*   Full Functional Dependency
    *   \[ X1, X2 ] → \[Y] if only know X1 or X2, cannot determine Y
*   Partial Functional Dependency
    *   \[ X1, X2 ] → \[Y] if only know X1, can determine Y, Y may have no relationship with X2
*   Transitive Functional Dependency
    *   \[X] → \[Y] and \[Y] → \[Z] then \[X] can determine \[Z] indirectly

### First Normal Form (1NF)

**Every attribute must be atomic.**

*   Atomicity: Each attribute must be a single value.
*   属性中无重复的域
*   拆分记录

### Second Normal Form (2NF)

**Every non-key attribute must be functionally dependent on the entire primary key.**

*   Non-key attribute: Attribute that is not a primary key.
*   消除部分依赖，对于复合主键
*   差分表格

### Third Normal Form (3NF)

**Every non-key attribute must be functionally dependent on the entire primary key, and must not be transitively
dependent on the primary key.**

*   消除非主键字段对主键的传递依赖
*   拆分成多个子表
