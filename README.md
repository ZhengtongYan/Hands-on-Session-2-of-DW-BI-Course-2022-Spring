# **Hands-on Session 2: Transaction Processing and Multi-Model OLAP Analysis with AgensGraph**

**This repository contains the instructions of the second hands-on session in our DW&BI course.**


## **Learning Objectives**
Gain hands-on experience in transaction processing and multi-model OLAP analysis using AgensGraph.
- Learn how to perform transaction processing in AgensGraph;
- Learn how tp write multi-model OLAP queries in AgensGraph.


## **Software Requirements**

You need AgensGraph database to complete the exercies. 

Windows and Linux users can directly download the installers and then install them based on the installation guide.

- **Windows system**: Download the [installer](https://bitnine.net/agensgraph-2-5-0-community-windows/)
and then install it based on the installation guide.

- **Linux system**: Download the [installer](https://bitnine.net/agensgraph-2-5-0-community-linux/) and then install it based on the installation guide.

- **Installation Guide**: Download the zip file from this [link](https://bitnine.net/agensgraph-2-5-0-installation-guide/), then unizp this file and you will get two pdf fiels:
  - agens_graph_linux_installation_guide_html.pdf
  - agens_graph_windows_installation_guide_html.pdf

MacOS users need to build from the source code to install the AgensGraph.

- **MacOS system**: Please refer to this [video](https://www.youtube.com/watch?v=o9bKSAVk1KQ) to install AgensGraph in MacOS.
  

## **Download the Dataset**

In part 2 of this hands-on session, you need to use three schema of the Unibench benchmark, including M3D (Multi-Model, MultiDimensional) schema, FR (Full Relational) schema, and NR (Non-Relational) schema. Note that part 1 (transaction processing) does not need to use Unibench.

**Download the dmp file:** Please download the dataset from [this link](https://github.com/big-unibo/m3d). Click "here" in the "Loading the data" section (see the following figure) to download the compressed dump file (m3d.dmp).

![image](https://github.com/ZhengtongYan/-Hands-on-Session-2-of-DW-BI-Course/blob/main/Dataset.PNG)


For more details about these three schemas of Unibench, please read Paper 2 in the references section.


## **References**
- **1. Two Papers**
  - **Paper 1: Unibench Benchmark**. Chao Zhang, and Jiaheng Lu. ["Holistic evaluation in multi-model databases benchmarking."](https://link.springer.com/article/10.1007/s10619-019-07279-6) Distributed and Parallel Databases 39, no. 1 (2021): 1-33.
  - **Paper 2: OLAP Analysis on Unibench.** Sandro Bimonte, Enrico Gallinucci, Patrick Marcel, and Stefano Rizzi. ["Data variety, come as you are in multi-model data warehouses."](https://www.sciencedirect.com/science/article/pii/S0306437921000090?casa_token=aADxHmon9woAAAAA:zXPlPVQckCh5dlzQ6Qe9nIrlojL3XA8hApPVIvoMkmR26drXSIJzNMkb2whT-RyfZQsbnB2zew) Information Systems (2021): 10173
  

- **2. AgensGraph Documentation**
  - [AgensGraph Quick Guide](https://bitnine.net/documentations/manual/quick_guide/agens_graph_quick_guide_html.pdf) 
  - [AgensGraph Developer Manual](https://bitnine.net/documentations/manual/developer/english/agens_graph_developer_manual_html.pdf)
  - [AgensGraph Operations Manual](https://bitnine.net/documentations/manual/operation/english/agens_graph_operation_manual_html.pdf)

  For Cypher query language, please refer to the AgensGraph Developer Manual.

- **3. PostgreSQL Documentation**
  - [JSON Functions and Operators](https://www.postgresql.org/docs/11/functions-json.html)
  - [XML Functions](https://www.postgresql.org/docs/11/functions-xml.html)
  - [hstore](https://www.postgresql.org/docs/11/hstore.html)

  Since AgensGraph 2.5.0 is based on PostgresSQL 11.11, so you may refer to the documentation of version 11.

- **4. Transaction Processing**

  - [Introduction of transaction isolation levels in PostgreSQL](https://www.postgresql.org/docs/current/transaction-iso.html)
  - [Introduction of locks in PostgreSQL](https://www.postgresql.org/docs/14/explicit-locking.html)
  - [Introduction of deadlocks in PostgreSQL](https://www.postgresql.org/docs/14/explicit-locking.html#LOCKING-DEADLOCKS)
  - [Transaction tutorial page for PostgreSQL](https://www.postgresqltutorial.com/postgresql-transaction/)


## **Exercises (20 points)**


## **Part1: Transaction Processing (5 points)**

#### **Initialize demo database**
```SQL
CREATE TABLE "accounts" (
  "id" bigserial PRIMARY KEY,
  "owner" varchar NOT NULL,
  "balance" bigint NOT NULL,
  "currency" varchar NOT NULL,
  "createdat" timestamptz NOT NULL DEFAULT (now())
);
```

```SQL
INSERT INTO accounts (owner, balance, currency)
VALUES
  ('AA', 100, 'EUR'),
  ('BB', 100, 'EUR'),
  ('CC', 100, 'EUR');
```

**1. Read Phenomena and Isolation Levels**

Consider a situation where the balance of AA's account (id=1) is initially 100 and two users simultaneously execute commands within transactions in AgensGraph:


| Transaction 1 (user 1) | Transaction 2 (user 2)| 
| -------- | -------- | 
| BEGIN;| |
| | BEGIN;|
| | UPDATE accounts SET balance=50 WHERE id=1;|
| SELECT balance FROM accounts WHERE id=1; | |
|  | UPDATE accounts SET balance=70 WHERE id=1; |
| | COMMIT;|
| SELECT balance FROM accounts WHERE id=1; | |
| COMMIT; | |


   (1) At the <font color=blue>READ COMMITTED</font> level, what results the user 1 can get from the two queries?  What read phenomenon occur? (1 point)

   (2) At the <font color=blue>REPEATABLE READ</font> level, what results the user 1 can get from the two queries?  What read phenomenon occur? (1 point)

   (3) At the <font color=blue>SERIALIZABLE</font> level, what results the user 1 can get from the two queries?  What read phenomenon occur? (1 point)

   (4) At the <font color=blue>READ UNCOMMITTED</font> level, what results the user 1 can get from the two queries if the transactions are executed in MySQL instead of AgensGraph? What read phenomenon occur? (Hint: In AgensGraph, READ UNCOMMITTED is treated as READ COMMITTED. But in MySQL, READ UNCOMMITTED is different from READ COMMITTED.) (1 point)

**4. Locks and Deadlock**

（1）What kinds of locks do the following two transactions try to generate in AgensGraph? Whether the two locks are conflicting or not? Will the records in the *accounts* table be deleted by the truncate command? (0.5 point)

| Transaction 1 | Transaction 2| 
| -------- | -------- | 
| BEGIN;| |
| | BEGIN;|
| | TRUNCATE accounts;|
| SELECT * FROM accounts;| |
|  | ROLLBACK; |
| COMMIT; |  |


(2）Will a deadlock occur between the following two concurrent transactions? Why? (0.5 point)

| Transaction 1 (user 1) | Transaction 2 (user 2)| 
| -------- | -------- | 
| BEGIN;| |
| | BEGIN;|
| | UPDATE accounts SET balance=150 WHERE id=1;|
| UPDATE accounts SET balance=150 WHERE id=2; | |
| UPDATE accounts SET balance=180 WHERE id=1; | |
| | UPDATE accounts SET balance=180 WHERE id=2;|





## **Part2: Multi-Model OLAP Analysis (Total: 15 points)**


Import the downloaded file "m3d.dmp" into AgensGraph using pg_restore command. Refer to this link about [pg_restore](https://www.postgresql.org/docs/current/app-pgrestore.html) command.

**Step 1:** In AgensGraph, create a database called "unibench_m3d" and a graph called "unibench_graph", and set the graph_path. The commands are as follows:
```SQL
CREATE DATABASE unibench_m3d;
CREATE GRAPH unibench_graph;
SET graph_path = unibench_graph;
```

**Step 2:** Open a new terminal and change the directory of "m3d.dmp" file, then input the following command to import the "m3d.dmp" dataset:
```SQL
pg_restore -d unibench_m3d -U agens -O -w -v m3d.dmp
```
**Notice:** You need to change the current directory into the one of the dataset, otherwise pg_restore cannot find the dataset. You can also specify the path of m3d.dmp in the pg_restore command, for example:
 ```SQL
pg_restore -d unibench_m3d -U agens -O -w -v /home/dw/m3d.dmp
``` 

**Notice:** You may see some errors about the <font color=red>pg_dropcache</font> EXTENSION, just ignore these errors because pg_dropcache is only an extension of PostgreSQL for invalidating shared_buffers cache, so it does not affect the imported results.

After importing the dataset, please complete the following 5 questions: Q1-Q5. For each question, you need to write three kinds of queries (FR, NR, and M3D) to answer the same question. Make sure that the three queries for the same question can get the same results. 
- FR: Full-Relation query is based on the FR schema;
- NR: Non-Relational query is based on the NR schema;
- M3D: Multi-Model Multidimensional query is based on the M3D schema.

Please read paper 2 and refer to this [Github](https://github.com/big-unibo/m3d) to see the query examples.

**Q1:** Number of orders by year.

**Q2:**  Number of orders by customer rating for a given product (asin='B0007QCO4M').

**Q3:** Number of orders by a customer's 2-degree (2 hops) friends. The idcust of the customer is 4145.

**Q4:** Total price by customer for a given vendor (vendorname='Mugen_Motorsports')
and period (2018-2020).

**Q5:** Total price by vendor for the top 3 customers,
order by vendor.



## **(Optional) Part3: Bouns of Multi-Model OLAP Analysis (Maximum: 5 points)**
In this part, you can get an extra bouns by designing at most five different multi-model OLAP questions and give the M3D query for each question. 

Requirments: 
- The queries should not be included in Paper 2 and Part 2. This means you cannot simiply modify some parameters of the queries in paper 2. Instead, you need to design queries that have different descriptions and semantics.
- Each query should be complex enough and consists of <font color=red>at least</font> four different data models (R, JSON, Graph, XML, KV). Simple queries cannot get the points.
- Please give the description of each query;
- Please show the involved data models in each query;
- You ONLY need to give the M3D query format. You DO NOT need to give the FR and NR query formats.

Please refer to Table 1 and Table 2 in Paper 2 (WL1 and WL2 OLAP workload for Unibench) to design the queries.


## **Submission Requirements**:  
- Post the queries and demonstrate the results.
- Please upload all the solutions in a single PDF file.
- Please submit to Moodle page and the deadline is **May 12th, 2022**.

