# **Hands-on Session 2: Transaction Processing and Multi-Model OLAP Analysis with AgensGraph**

**This repository contains the instructions of the second hands-on session in our DW&BI course.**


## **Learning Objectives**
Gain hands-on experience in transaction processing and multi-model OLAP analysis using AgensGraph.
- Learn how to perform transaction processing in AgensGraph;
- Learn how write multi-model OLAP queries in AgensGraph.


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

**Dowanload the dmp file:** Please download the dataset from [this link](https://github.com/big-unibo/m3d). Click "here" in the "Loading the data" section (see the following figure) to download the compressed dump file of the database.

![image](https://github.com/ZhengtongYan/-Hands-on-Session-2-of-DW-BI-Course/blob/main/Dataset.PNG)


For more details about these three schemas of Unibench, please refer to paper 2 in the references section.


## **References**
- **1. Two Papers**
  - **Paper 1: Unibench Benchmark**. Chao Zhang, and Jiaheng Lu. ["Holistic evaluation in multi-model databases benchmarking."](https://link.springer.com/article/10.1007/s10619-019-07279-6) Distributed and Parallel Databases 39, no. 1 (2021): 1-33.
  - **Paper 2: OLAP Analysis on Unibench.** Sandro Bimonte, Enrico Gallinucci, Patrick Marcel, and Stefano Rizzi. ["Data variety, come as you are in multi-model data warehouses."](https://www.sciencedirect.com/science/article/pii/S0306437921000090?casa_token=aADxHmon9woAAAAA:zXPlPVQckCh5dlzQ6Qe9nIrlojL3XA8hApPVIvoMkmR26drXSIJzNMkb2whT-RyfZQsbnB2zew) Information Systems (2021): 10173
  

- **2. AgensGraph Documentation**
  - [AgensGraph Quick Guide](https://bitnine.net/documentations/manual/quick_guide/agens_graph_quick_guide_html.pdf) 
  - [AgensGraph Developer Manual](https://bitnine.net/documentations/manual/developer/english/agens_graph_developer_manual_html.pdf)
  - [AgensGraph Operations Manual](https://bitnine.net/documentations/manual/operation/english/agens_graph_operation_manual_html.pdf)

- **3. PostgreSQL Documentation**
  - [PostgreSQL Documentation](https://www.postgresql.org/docs/current/)
