
---

## ✅ Stage 2: Apache Spark Essentials (Deep Dive)

---

### 🔹 1. Spark Architecture

#### 💡 What is Apache Spark?

Apache Spark is an open-source, distributed processing engine optimized for large-scale data processing. It enables fast, in-memory computations using a DAG (Directed Acyclic Graph) execution engine.

#### 🧱 Core Components:

| Component           | Role                                                     |
| ------------------- | -------------------------------------------------------- |
| **Driver**          | Orchestrates the execution; runs your Spark application  |
| **Cluster Manager** | Allocates resources (e.g., Databricks, YARN, Kubernetes) |
| **Executors**       | Perform the actual computation on data                   |
| **Tasks**           | Units of work sent to Executors                          |
| **DAG**             | Execution plan Spark builds from transformations         |

#### 🧠 Execution Flow:

1. You write a Spark job in Python/Scala.
2. Spark driver creates a **logical execution plan** (DAG).
3. The DAG is split into **stages** and **tasks**.
4. Tasks are distributed across **executors** on the cluster.

---

### 🔹 2. Spark Core Concepts

#### ✅ **RDD (Resilient Distributed Dataset)**

Low-level, fault-tolerant distributed collection of objects. Rarely used directly in Databricks today.

#### ✅ **DataFrame**

* Distributed table with named columns.
* Built on top of RDDs.
* Most commonly used abstraction.

#### ✅ **Dataset** (Scala/Java only)

* Type-safe version of DataFrames (not commonly used in Python).

#### 🔁 **Transformations vs Actions**

| Type                | Examples                                  | Description                 |
| ------------------- | ----------------------------------------- | --------------------------- |
| **Transformations** | `filter()`, `map()`, `select()`, `join()` | Lazy operations — build DAG |
| **Actions**         | `show()`, `count()`, `collect()`          | Triggers execution          |

#### 🔧 **SparkSession**

* Entry point to any Spark code.

```python
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName("MyApp").getOrCreate()
```

---

### 🔹 3. PySpark / DataFrame API

Databricks primarily uses PySpark or Spark SQL under the hood. Here's what you should learn to become fluent:

#### 📥 Reading Data:

```python
df = spark.read.format("csv").option("header", "true").load("/path/to/file.csv")
```

#### 📤 Writing Data:

```python
df.write.mode("overwrite").parquet("/path/output")
```

#### ✂️ Basic Transformations:

```python
df.select("name", "age")
df.filter(df["age"] > 30)
df.groupBy("department").agg({"salary": "avg"})
df.withColumn("new_col", df["old_col"] * 2)
```

#### 🔁 Join Example:

```python
df1.join(df2, df1.id == df2.emp_id, "inner")
```

#### 📌 Schema Inference:

```python
df.printSchema()
df.describe().show()
```

#### 📊 Actions:

```python
df.show()
df.count()
df.collect()
```

---

### 🔄 Real-World Analogy for Spark:

> Imagine you're a manager (Driver), delegating tasks to teams (Executors). Each team processes part of the workload (data partitions). You define the workflow (DAG), but work doesn’t start until you say "Go!" (Action).

---

### ✅ Summary Checklist for Stage 2:

* [ ] Understand Spark architecture: driver, executor, DAG
* [ ] Know the difference: RDD vs DataFrame
* [ ] Write basic PySpark code
* [ ] Load, transform, and save data using DataFrame API
* [ ] Perform joins, filters, and aggregations

---

