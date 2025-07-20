
---

## ✅ Stage 3: Databricks Runtime & Cluster Management (Deep Dive)

---

### 🔹 1. Cluster Types in Databricks

Clusters are **virtual machines** that run your notebooks, jobs, or libraries. They execute your Spark code.

#### 🛠️ Types of Clusters:

| Cluster Type            | Description                                                  |
| ----------------------- | ------------------------------------------------------------ |
| **Interactive Cluster** | Used manually for notebooks, development, and ad hoc queries |
| **Job Cluster**         | Automatically spun up to run a scheduled job/task            |
| **All-Purpose Cluster** | Can be shared among users for collaborative work             |
| **Single Node Cluster** | Useful for development, small workloads, and prototyping     |

---

#### 🔧 Key Cluster Configuration Settings:

* **Databricks Runtime Version**
  Choose based on workload (latest with Delta Lake is preferred).

* **Worker Type & Driver Type**
  Choose VM instance size (e.g., `Standard_DS3_v2` on Azure).

* **Autoscaling**
  Enable to dynamically adjust number of workers based on load.

* **Termination Settings**
  Always set idle timeout to auto-terminate unused clusters.

---

### 🔹 2. Library Management in Databricks

Databricks supports installation of libraries for both **interactive** and **job-based** clusters.

#### 📦 Ways to Install Libraries:

| Method               | Source              | Use Case                                  |
| -------------------- | ------------------- | ----------------------------------------- |
| **PyPI**             | `pypi.org`          | Python packages (`pandas`, `numpy`, etc.) |
| **Maven**            | Java/Scala packages | Spark connectors, JDBC drivers            |
| **CRAN**             | R libraries         | If using R                                |
| **Upload .whl/.jar** | Custom packages     | Internal code dependencies                |

#### 🔍 Scope of Libraries:

* **Cluster-scoped**: Installed to the cluster, available across notebooks.
* **Notebook-scoped**: Installed within a notebook using `%pip` or `%conda` magic.

```python
# Notebook-scoped example:
%pip install pandas
```

---

### 🔹 3. Databricks File System (DBFS)

DBFS is a **distributed file system** abstraction over cloud storage (S3, ADLS, or GCS).

#### 📁 Structure:

| Path Example          | Description                           |
| --------------------- | ------------------------------------- |
| `dbfs:/mnt/`          | Mounted cloud storage                 |
| `dbfs:/FileStore/`    | Used for uploads, images, small files |
| `file:/`              | Local filesystem of the driver node   |
| `/databricks/driver/` | Scratch area used by the notebook     |

#### 🔧 Common Operations:

```python
# Upload a file manually in UI or programmatically
dbutils.fs.cp("file:/tmp/data.csv", "dbfs:/FileStore/data.csv")

# List files
dbutils.fs.ls("dbfs:/FileStore/")

# Read file using Spark
df = spark.read.option("header", True).csv("dbfs:/FileStore/data.csv")
```

---

### 📘 Real-World Analogy:

> Think of a **cluster** as a car. You pick the engine (instance size), configure the GPS (runtime version), pack your tools (libraries), and drive on the road (DBFS paths). For a delivery (job), you may rent a car just for that one trip (job cluster).

---

### ✅ Summary Checklist for Stage 3:

* [ ] Understand cluster types and when to use them
* [ ] Launch and configure a cluster
* [ ] Install and manage libraries using PyPI or Maven
* [ ] Navigate DBFS and access files for reading/writing

---

