
---

## ✅ Stage 4: Delta Lake (Deep Dive)

### 🔹 What is Delta Lake?

Delta Lake is an **open-source storage layer** that brings **ACID transactions**, **schema enforcement**, and **time travel** to your data lakes. It enables you to build **reliable data lakes** that behave like traditional databases.

#### 🚀 Why It Matters:

* Avoids corrupted or inconsistent data from failed jobs
* Enables rollback and auditing (time travel)
* Supports incremental data loads with **MERGE**, **UPDATE**, **DELETE**
* Supports concurrent reads and writes safely

---

### 🔹 1. Delta Lake Basics

#### 💡 Delta Format:

Internally stores **Parquet files** + **transaction logs** (`_delta_log/`)
Each table is a directory with:

* Parquet data files
* JSON logs to track commits and changes

#### 🛠️ Converting to Delta:

```python
# Read CSV and save as Delta
df = spark.read.csv("/mnt/input.csv", header=True)
df.write.format("delta").mode("overwrite").save("/mnt/delta-table")
```

---

### 🔹 2. Delta Table Operations

#### 📗 Create a Delta Table

**Method 1: Using PySpark**

```python
df.write.format("delta").save("/mnt/my-table")
```

**Method 2: Using SQL**

```sql
CREATE TABLE my_table
USING DELTA
LOCATION '/mnt/my-table';
```

---

#### 📘 Read from Delta Table:

```python
df = spark.read.format("delta").load("/mnt/my-table")
df.show()
```

---

#### ✍️ Upsert (MERGE INTO):

Upsert = Update if matched, insert if not

```sql
MERGE INTO target USING source
ON target.id = source.id
WHEN MATCHED THEN UPDATE SET *
WHEN NOT MATCHED THEN INSERT *
```

Or using PySpark:

```python
from delta.tables import DeltaTable

deltaTable = DeltaTable.forPath(spark, "/mnt/my-table")
deltaTable.alias("tgt").merge(
    sourceDF.alias("src"),
    "tgt.id = src.id"
).whenMatchedUpdateAll().whenNotMatchedInsertAll().execute()
```

---

#### ❌ DELETE and UPDATE:

```sql
DELETE FROM my_table WHERE status = 'inactive';

UPDATE my_table SET salary = salary * 1.1 WHERE department = 'HR';
```

---

### 🔹 3. Time Travel

Allows you to query previous versions of your Delta table.

#### 🕒 Query Past Versions:

```python
# Using version number
df = spark.read.format("delta").option("versionAsOf", 2).load("/mnt/my-table")

# Using timestamp
df = spark.read.format("delta").option("timestampAsOf", "2024-12-01T10:00:00").load("/mnt/my-table")
```

---

### 🔹 4. Performance Optimizations

#### 🧹 Vacuum (Remove Old Files):

```sql
VACUUM my_table RETAIN 168 HOURS
```

**Caution:** Vacuum removes files no longer in the Delta log; don’t run it before you’re done with time travel.

---

#### 🚀 OPTIMIZE and ZORDER:

Used to **compact** files and **improve query speed**.

```sql
OPTIMIZE my_table ZORDER BY (customer_id)
```

> ZORDER organizes data on disk to speed up WHERE clauses on the selected column.

---

### 🧠 Real-World Analogy:

> Think of Delta Lake as a **version-controlled Excel sheet** in a shared drive. You can insert, update, delete rows; see change history; and revert back if someone messed up the sheet.

---

### ✅ Summary Checklist for Stage 4:

* [ ] Create and query Delta tables
* [ ] Perform UPSERT, DELETE, UPDATE operations
* [ ] Use time travel to read historical data
* [ ] Understand VACUUM and OPTIMIZE for cleanup and speed

---


