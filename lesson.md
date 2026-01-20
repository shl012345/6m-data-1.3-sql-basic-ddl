# **📘 Lesson Plan: SQL Data Definition (DDL) with DuckDB**
**In data science, data is the new oil. DDL is how we build the refinery.**

* **Module:** 1.3 Data Definition & Management 
* **Target Audience:** Adult Learners (Basic Python knowledge) 
* **Methodology:** Flipped Classroom / Code-along / "I do, We do, You do"
* **Tools:** [DbGate](https://dbgate.org/), [DuckDB](https://duckdb.org/)

## **🎯 Learning Objectives (Bloom's Taxonomy)**

By the end of this session, learners will be able to:

1. **Identify** the hierarchical structure of a relational database (Database \> Schema \> Table) to organize data effectively.  
2. **Apply** `CREATE` statements with strict constraints (Primary/Foreign Keys) to **construct** a relational schema that enforces data integrity.  
3. **Execute** data pipeline commands (`COPY`) to **import** raw CSV data into structured tables for analysis.

## **⏱️ Agenda & Topic Prioritization**

| Time | Section | Focus | Top 3 Core Topics (Must Cover) | Optional Topics (Mention briefly or Skip) |
| ----- | ----- | ----- | ----- | ----- |
| **0:00 \- 0:55** | **1\. The Foundation** | **Identify** | **`CREATE TABLE` & Types:** Defining the physical container for data and choosing the right `VARCHAR` vs `INT`. | DuckDB internal storage (`.wal` files), Sequence objects. |
| **0:55 \- 1:05** | *Break* |  |  |  |
| **1:05 \- 2:00** | **2\. The Blueprint** | **Construct** | **Constraints (PK/FK):** The "Rules" of the database. How `PRIMARY KEY` and `FOREIGN KEY` prevent bad data. | Complex `CHECK` constraints (Regex), Composite Keys. |
| **2:00 \- 2:10** | *Break* |  |  |  |
| **2:10 \- 3:00** | **3\. The Pipeline** | **Execute** | **Data Import (`COPY`):** Moving external data (CSV) into the database to make it useful. | `DROP` vs `TRUNCATE`, Exporting to JSON. |





---
## **Section 1: The Foundation (Structure)**

**Goal:** Move from "What is a database?" to "I have a place to store data."

#### **1.1 The Narrative**

"In Python, you create variables like `my_data = []`. But when you turn off the computer, that data vanishes. To make data specific and permanent, we need a **Database**. Think of it like a physical File Cabinet:

* **The Database:** The Cabinet itself.  
* **The Schema:** A specific drawer (labeled 'HR' or 'Sales').  
* **The Table:** A folder inside that drawer.  
* **The Column:** The specific form fields in that folder (Name, Date, ID). Today, we aren't just coding; we are building the cabinet."

#### **1.2 Concept: Types Matter**

* **INTEGER:** Math-able numbers. (e.g., Age, Count).  
* **VARCHAR:** Text. (e.g., Name, Email).  
* **DATE:** Chronological points. (Don't store dates as text\!)

---

We will be using two modern, lightweight tools:

1. **DuckDB:** A fast, "in-process" database engine. Unlike traditional databases (like MySQL), DuckDB doesn't require a complex server setup. It stores everything in a single file on your computer, making it perfect for data science.  
2. **DbGate:** A user-friendly database manager. It provides a visual interface to see your tables, run queries, and manage your data.



---
#### **🟢 Activity 1: The "Profile" Table (Code-along) (20 Mins)**
   
1. **Connecting to the Database:**
  
  * You can connect to any database using an SQL client or IDE like DbGate, which supports a broad range of popular SQL and NoSQL databases, including MySQL, PostgreSQL, SQL Server, MongoDB, SQLite, Oracle, Redis, CockroachDB, MariaDB, Amazon Redshift, ClickHouse, Cassandra, DuckDB, Firebird, and Firestore, with some premium features for cloud databases like Azure. It acts as a universal manager, allowing you to connect to and work with multiple database types from a single application.

  * For this lesson, we'll use DuckDB, a lightweight, in-process SQL database engine.

  * Open **DbGate**.  
  
  * Create a new connection to the DuckDB file provided in this repo.
     
```
   db/unit-1-3.db
```

> If you expand on the `unit-1-3.db`, you should see a few predefined schemas.  
> * The default schema is `main`.  
> * Any tables you create without specifying a schema will be in `main`.  
> * You can also create additional schemas to organize your tables.



2. **Creating a Schema:**

   We start by creating a new schema `lesson` to organise our tables.

```sql
CREATE SCHEMA lesson;
```

  * alternatively, we can also use the following to create a schema if it does not exist yet

```sql
CREATE SCHEMA IF NOT EXISTS lesson;
```


3. **Creating your first Table:**

  * We will be creating a table `users` in the `lesson` schema. The table will have the following columns:

- `id` - integer
- `name` - varchar
- `email` - varchar
  
```sql
CREATE TABLE lesson.users (  
  id INTEGER,  
  name VARCHAR,  
  email VARCHAR  
);
```

> If you just want to create tables in the default (`main`) schema, you can omit the `lesson.` prefix.


4. **Basic Data Entry (DML basics for testing DDL):**

  * We can insert data into the table using the `INSERT INTO` statement.
   
```sql
INSERT INTO lesson.users (id, name, email)  
VALUES (1, 'John Doe', 'john.doe@gmail.com');
```

  * This will insert a new row / record into the `users` table. You can insert multiple rows at once.

```sql
INSERT INTO lesson.users (id, name, email)
VALUES (2, 'Jane Doe', 'jane.doe@gmail.com'),
       (3, 'John Smith', 'john.smith@gmail.com');
```

> Insert two more rows with contiguously increasing `id` values, random `name`s and `email`s.

5. **Alter Tables**

  * We can alter the tables to add, rename or remove columns.

  * Add column 'start_date' to table users.

```sql
ALTER TABLE lesson.users ADD COLUMN start_date DATE;
```

Rename column 'id' to 'uid' in table classes.

```sql
ALTER TABLE lesson.users RENAME id TO uid;
```

6. **TRUNCATE (Emptying the table) vs. DROP (Deleting the table):**



```sql
ALTER TABLE lesson.users
DROP COLUMN email;
```

```sql
TRUNCATE TABLE lesson.users;
```

```sql
DROP TABLE lesson.users;
```
> **Tip: "Always run a SELECT before a DROP. Double-check your table name. It’s better to be slow and correct than fast and sorry."**

* **Q\&A:** Addressing connection issues and terminology (Database vs. Schema).  
* **Reflection:** Why is organizing data into schemas important in a production environment?

---
## **Section 2: The Blueprint - Building Relationships & Constraints**

**Goal:** Translate a diagram (ERD) into code that enforces rules.

#### **2.1 The Narrative**

"In Excel, you can type 'Banana' into a column meant for Prices. Excel doesn't care. Databases *do* care. We are the architects, and we need to install 'Bouncers' at the door. These bouncers are called **Constraints**. If you try to enter a duplicate User ID? **Blocked.** If you try to enter a Class for a Teacher that doesn't exist? **Blocked.**"

#### **2.2 Concept: The Keys**

* **Primary Key (PK):** The Fingerprint. Unique and Not Null.  
* **Foreign Key (FK):** The Link. It points to a PK in another table.

* **Theory Summary/Recap:**  
  * Constraints: Primary Keys (Uniqueness), Foreign Keys (Relationships), NOT NULL, CHECK, and DEFAULT.  
  * Understanding the School System ERD (Students, Teachers, Classes).

<details>
  <summary>Primary Keys, Foreign Keys, NOT NULL, CHECK, and DEFAULT</summary>
  
  * `primary key` or `unique` define a column, or set of columns, that are a unique identifier for a row in the table.
  * `primary key` constraints and unique constraints are identical except that a table can only have one primary key constraint defined, but many unique constraints and a primary key constraint also enforces the keys to not be NULL.
  * `foreign key` defines a column, or set of columns, that refer to a primary key or unique constraint from another table. The constraint enforces that the key exists in the other table.
  * `not null` specifies that the column cannot contain any NULL values. By default, all columns in tables are nullable.
  * `default` specifies a default value for the column when no value is specified.
  * `check` constraint allows you to specify an arbitrary boolean expression. Any columns that do not satisfy this expression violate the constraint.
</details>

#### **🟢 Activity 2: The "Missing Link" Challenge (25 Mins)**

**Scenario:** The Instructor builds the `teachers` and `classes` tables. The students must independently build the `students` table based on the ERD.

**Part A: Instructor Demo (Code-along)**
 
**Creating Tables with Constraints:**  
```sql
CREATE TABLE lesson.teachers (
  id INTEGER PRIMARY KEY, -- primary key
  name VARCHAR NOT NULL, -- not null
  age INTEGER CHECK(age > 18 AND age < 70), -- check
  address VARCHAR,
  phone VARCHAR,
  email VARCHAR CHECK(CONTAINS(email, '@')) -- check
);

CREATE TABLE lesson.classes (
  id INTEGER PRIMARY KEY, -- primary key
  name VARCHAR NOT NULL, -- not null
  teacher_id INTEGER REFERENCES lesson.teachers(id) -- foreign key
);
```

**Part B: Learner Challenge**

Complete the CREATE TABLE statement for the students table based on the ERD (Entity-Relationship Diagram) below.

> In a school system whose classes have students and teachers. Each student belongs to a single class. Each teacher may teach more than one class, but each class only has one teacher.

```dbml
Table students {
  id int [pk]
  name varchar
  address varchar
  phone varchar
  email varchar
  class_id int
}

Table teachers {
  id int [pk]
  name varchar
  age int
  address varchar
  phone varchar
  email varchar
}

Table classes {
  id int [pk]
  name varchar
  teacher_id int
}

Ref: students.class_id > classes.id // many-to-one

Ref: classes.teacher_id > teachers.id // many-to-one
```

* **Q\&A:** Troubleshooting foreign key errors and understanding "CHECK" logic.  
* **Reflection:** How do constraints prevent "bad data" from entering your analysis pipeline?


---
## **Section 3: The Pipeline - Data Management & Performance**

**Goal:** Manage data efficiency and bulk loading.

#### **3.1 The Narrative**

"We have empty tables. Typing `INSERT INTO...` one row at a time is fine for 10 rows. It is impossible for 1 million rows. As Data Scientists, you will often receive a 'dump'—a massive CSV file from a client. You need to get that into your SQL environment instantly. We use the **COPY** command. It is the industrial vacuum cleaner of SQL."

#### **3.2 Concept: Views vs. Tables**

* **Table:** Physical storage. Takes up hard drive space.  
* **View:** A "Saved Search", a virtual query, it runs every time you look at it. Good for simplifying complex joins.
* **Indexes** Think of a book index for speed. 
  

<details>
  <summary>What are Indexes?</summary>
  
  * Indexes are used to improve the performance of queries. They are not required but are recommended for tables with many rows. They are used to _retrieve data from the database more quickly than otherwise_. Indexes are created using one or more columns of a database table. The users cannot see the indexes, they are just used to speed up searches/queries.
</details>

<details>
  <summary>Tables vs. Views</summary>
  
  * Tables and views are both ways to store data. What is the difference between them? A table is a physical copy of the data (materialized), while a view is a virtual copy of the data. A view is a query that is run on the fly when you access the view. A view is not stored in the database, but the query that defines the view is stored in the database.
</details>



#### **🟢 Activity 3: The CSV Dump (Real-World Task, Code-along)**

1. **Importing Data:**

  * We can import data from a CSV file into a table.

```sql
COPY lesson.teachers FROM '<full directory path>' (AUTO_DETECT TRUE);
```

> **Note:** For data import, use the full directory path to the CSV files, e.g. `/Users/fengfeng/Dev/6m-data-1.3-sql-basic-ddl/data/teachers.csv`

2. **Exercise:**

  * Import data for `classes` and `students` tables. 


3. **Updating Data:**

  * We can update the data in the table using the `UPDATE` statement.

  * Let's say `Linda Garcia` changed her email to `linda.g@example.com`, we can update the `email` of the student with id 4 (her id) to `linda.g@example.com`. The `WHERE` clause is used to specify which rows to update.

```sql
UPDATE lesson.students
SET email = 'linda.g@example.com'
WHERE id = 4;
```
  > This is DML, we will learn more in next lesson. 

4. **Exporting Data:**

  * Let's export the data from the student table into a CSV file delimited with `|`.       Remember to prepend the full directory path to the CSV file.

```sql
COPY (SELECT * FROM lesson.students) TO '<full directory path>' WITH (HEADER 1, DELIMITER '|');

-- example <full directory path>: /Users/fengfeng/Dev/6m-data-1.3-sql-basic-ddl/data/students_new.csv
```

  * We can also export the data into a JSON file (you will learn more about JSON in Module 2).

```sql
COPY (SELECT * FROM lesson.students) TO '<full directory path>';

-- example <full directory path>:  /Users/fengfeng/Dev/6m-data-1.3-sql-basic-ddl/data/students.json
```

5. **Learner Challenge**

  * Repeat the above steps for the `teachers` & `classes` tables.

#### **🟢 Activity 4: Index, Table & View (Code-along)**

6. **Creating Indexes:**

   <details>
   <summary>Indexes</summary>
  
   * Indexes are used to improve the performance of queries. They are not required but are recommended for tables with many rows. They are used to _retrieve data from the database more quickly than otherwise_. Indexes are created using one or more columns of a database table. The users cannot see the indexes, they are just used to speed up searches/queries.
   </details>

```sql
-- Create a unique index 'teachers_name_idx' on the column name of table teachers.

CREATE UNIQUE INDEX teachers_name_idx ON lesson.teachers(name);
```

```sql
-- Create index 'students_name_idx' that allows for duplicate values on the column name of table students.

CREATE INDEX students_name_idx ON lesson.students(name);
```

```sql
-- View indexes for a specific table
SELECT index_name, sql 
FROM duckdb_indexes 
WHERE table_name = 'students';
```

7. **Tables vs Views:**

   <details>
   <summary>Tables and Views</summary>
  
   * Tables and views are both ways to store data. What is the difference between them? A table is a physical copy of the data (materialized), while a view is a virtual copy of the data. A view is a query that is run on the fly when you access the view. A view is not stored in the database, but the query that defines the view is stored in the database.
   </details>

  * We will be creating a view `students_view` in the `lesson` schema. The view will have the following columns:

  - `id` - integer
  - `name` - varchar
  - `email` - varchar

```sql
CREATE VIEW lesson.students_view AS
SELECT id, name, email
FROM lesson.students;
```

8. **Learner Challenge**

  * Create a view `teachers_view` with the same columns as `students_view` but for the `teachers` table.
   


* **Q\&A:** When should you *not* use an index? Difference between dropping a table vs. deleting data.  
  *Ans: small tables, tables not frequently updated, tables with high update volume, read-once table*
* **Reflection:** How does using a View simplify the work for a Data Analyst who only needs specific columns?



---

**Solution:**
#### **🟢 Activity 2: The "Missing Link" Challenge**
```dbml
CREATE TABLE lesson.students (
    id INTEGER PRIMARY KEY,
    name VARCHAR,
    email VARCHAR,
    class_id INTEGER REFERENCES lesson.classes(id) -- Did they remember this?
);
```

