# **Pre-Study Material: Introduction to SQL Data Definition Language (DDL)**

Welcome to the Data Science Bootcamp\! To make the most of our 3-hour live session, please review this material beforehand. This "flipped classroom" approach allows us to focus our live time on hands-on practice and troubleshooting.

## **1\. Core Concepts: What is a Database?**

Think of a database as a highly organized digital filing cabinet. In Data Science, we primarily use **Relational Databases (RDBMS)**.

* **Database:** The container for everything (e.g., "School\_System").  
* **Schema:** A folder inside the database used to group related items (e.g., "Enrollment\_Data").  
* **Table:** A specific spreadsheet within a schema (e.g., "Students").  
* **Columns (Fields):** The categories of data (e.g., Name, Age, Email).  
* **Rows (Records):** Individual entries (e.g., "John Doe, 25, john@example.com").

### **What is DDL?**

**Data Definition Language (DDL)** is the subset of SQL used to **build and modify the structure** of the database. While other SQL parts handle data entry, DDL creates the "skeleton."

### Video introduction
Watch this - 🎬 [SQL DDL - The Blueprint for Data](https://youtu.be/adACCuMFjac)

## **2\. Our Toolkit**

[DuckDB](https://duckdb.org/)
<details>
  <summary>Introduction to DuckDB: Why we use an in-process engine for data science.</summary>
  
### DuckDB is an in-process, analytical database management system (DBMS) specifically designed to address the needs of data scientists by bringing the database closer to the data itself. This approach is favored in data science for several key reasons: 
#### Why an In-Process Engine is Beneficial for Data Science
  * Zero-Copy Data Transfer: Data science often involves rapidly iterating on large datasets. DuckDB runs within the same process as the application (e.g., a Python script or R session), eliminating the overhead of inter-process communication or network transfers. This "zero-copy" data transfer drastically improves performance and efficiency when working with local data.
  * Ease of Deployment and No Server Management: As an in-process system, DuckDB does not require a separate server to be installed, configured, or managed. This simplifies the setup, making it an excellent tool for local analysis, laptop use, and environments where users lack administrative privileges or the need for a dedicated data warehouse team.
  * High Performance for Analytical Workloads: DuckDB is optimized for Online Analytical Processing (OLAP) queries, which are common in data science. It employs columnar storage and advanced query optimization techniques, enabling it to efficiently handle the large, complex, and often ad-hoc queries characteristic of data analysis.
  * Seamless Integration with Data Science Ecosystems: DuckDB offers robust and native integrations with popular data science tools and languages, such as Python (via the duckdb library), R, and Java. It can directly query data frames in memory and read from various file formats (like CSV, Parquet, and JSON), streamlining data ingestion and manipulation within existing workflows.
  * Portability and Reproducibility: Because the database runs within the application and the data can be stored in a single file, analyses conducted with DuckDB are highly portable. This simplifies sharing reproducible research and results, as the entire environment and data can be easily packaged and moved. 
  * In summary, DuckDB's design as an in-process, analytical database provides data scientists with a fast, easy-to-use, and highly integrated tool that removes common bottlenecks associated with traditional client-server database architectures. Users can learn more about its features and get started with the official DuckDB documentation and explore its capabilities. 

</details>

[DbGate](https://dbgate.org/)
<details>
  <summary>Introduction to DbGate: A cross-platform database manager.</summary>
  
### DbGate is a free, cross-platform database manager and data editor for SQL and NoSQL databases (MySQL, PostgreSQL, MongoDB, etc.), available as a desktop app (Win/Mac/Linux) or a web app, offering features like data browsing/editing (Excel-like), visual query design, schema comparison, data visualization, and AI-powered SQL chat for simplified database management. It's designed for ease of use, allowing quick data interaction in the browser or desktop with powerful tools for developers and DBAs. 
#### Key Features
  * Unified Interface: Manage various databases (SQL, NoSQL) from one tool.
  * Data Editing: Edit data directly in tables with Excel-like features (copy/paste, sorting, filtering).
  * Querying: Includes a query console with auto-completion, visual query designer, and AI chat for generating SQL.
  * Data Visualization: Create charts and ER diagrams from your data.
  * Import/Export: Supports multiple formats (CSV, JSON, XML, Excel, etc.).
  * Cross-Platform: Works on Windows, macOS, Linux, and in a web browser.
  * Open Source: Free under GPL-3.0 license for most uses, with premium options available. 
#### How it Works
  * Desktop App: A native application for your computer.
  * Web App: Can run in a browser (via Docker or NPM) or be self-hosted as an ASP.NET Core application for browser access. 
</details>

## **3\. Basic SQL Syntax to Know**

You don't need to memorize these yet, but try to understand what they do:

### **Creating Structure**

* CREATE SCHEMA: Creates a new "folder" in the database.  
* CREATE TABLE: Defines a new table and its columns.  
* INTEGER, VARCHAR, DATE: These are **Data Types**. They tell the database if a column should store numbers, text, or dates.

### **Managing Structure**

* ALTER TABLE: Used to add or change columns in an existing table.  
* DROP TABLE: Deletes a table entirely. **(Be careful: this cannot be undone\!)**

## **4\. Setup Instructions (Action Required)**

Please complete these steps before the live session:

1. **Install DbGate:** Download and install the version for your operating system from [dbgate.org](https://dbgate.org/).  
2. **Download the Database File:** Ensure you have unit-1-3.db file saved on your machine (the file is in https://github.com/su-ntu-ctp/6m-data-1.3-sql-basic-ddl/blob/main/db/).  

## **5\. Check Your Understanding**

Try to answer these questions mentally:

1. If I want to add a "Phone Number" column to an existing table, which command would I use?  
   *Ans: Alter table*
3. What is the difference between a Schema and a Table?  
   *Ans: Schema is like a folder/diagram of groups of related data files and a table is like a file of a specific data set of fields and records.*
4. Why might a Data Scientist prefer DuckDB over a heavy database server?  
   *Ans: DuckDB is preferred as it can run the database system standalone on a single computer vs the complex setup of a database server.*

**Questions?** Bring them to the Q\&A session at the start of our live class\!
