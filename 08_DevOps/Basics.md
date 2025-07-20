
---

## ✅ Stage 8: DevOps & Governance in Databricks (Deep Dive)

---

### 🔹 1. Unity Catalog (Data Governance & Access Control)

**Unity Catalog** is Databricks' centralized solution for **data governance**, offering **fine-grained access control**, **data lineage**, and **audit logging**.

#### 🛠️ Key Features:

| Feature                  | Purpose                                          |
| ------------------------ | ------------------------------------------------ |
| **Centralized Metadata** | Shared across workspaces                         |
| **Fine-Grained Access**  | Row-, column-level permissions                   |
| **Data Lineage**         | Tracks which notebooks/queries used which tables |
| **Table Auditing**       | Who accessed what data, and when                 |

#### 🧰 Setup Includes:

* Assigning **metastores** per region
* Creating **catalogs**, **schemas (databases)**, and **tables**
* Managing access using **GRANT/DENY** on Unity Catalog objects

#### 🔐 Example:

```sql
GRANT SELECT ON TABLE finance.transactions TO `data_analyst_group`
```

> Think of Unity Catalog as the **Active Directory + Audit Log** of your data platform.

---

### 🔹 2. Git Integration with Repos

Databricks supports **native Git integration** via the **Repos** feature.

#### 💡 Supported Systems:

* GitHub
* GitLab
* Bitbucket
* Azure DevOps

#### 📦 Use Cases:

* Version control of notebooks and pipelines
* Collaborative development
* Pull requests and code reviews

#### 🛠️ How to Use:

1. Go to **Repos** tab
2. Connect to your Git repository
3. Clone and work with notebooks
4. Push, pull, and commit via the UI or `%git` commands

---

### 🔹 3. CI/CD for Databricks

Automate deployments and testing using your preferred CI/CD pipeline.

#### 🛠️ Common Tools:

| Tool                       | Use                                                           |
| -------------------------- | ------------------------------------------------------------- |
| **GitHub Actions**         | Push/pull notebooks, run jobs                                 |
| **Azure DevOps Pipelines** | Deploy jobs, clusters                                         |
| **Terraform**              | Infrastructure as code (clusters, jobs, permissions)          |
| **Databricks CLI**         | Interact programmatically with workspace (sync, deploy, test) |

#### Example CI/CD Flow:

1. Developer pushes notebook to Git
2. CI tool triggers Databricks CLI or REST API
3. Code is tested (unit tests, validations)
4. If passed, job is deployed and scheduled

#### 🧪 CLI Commands:

```bash
databricks workspace import_dir ./src /Workspace/ProjectX
databricks jobs create --json-file job_definition.json
```

---

### 🔹 4. Monitoring & Logging

#### 🔍 Key Areas:

* **Job runs**: Status, duration, logs
* **Cluster logs**: Spark events, GC logs
* **SQL queries**: Execution plans, cache usage

#### 📊 Tools:

* **Databricks UI**: For logs and job history
* **Audit logs**: Enterprise feature, exportable to SIEM (e.g., Splunk)
* **REST API**: Automate monitoring and alerting

#### 📘 Example: View logs

```python
dbutils.fs.head("dbfs:/databricks/jobs/job-12345/run-1/driverlog.txt")
```

---

### 🔹 5. Secrets & Credentials Management

Manage secrets like API keys or passwords using **Databricks Secret Scopes**.

#### 💡 Example:

```bash
databricks secrets create-scope --scope prod-secrets
databricks secrets put --scope prod-secrets --key db-password
```

In notebooks:

```python
dbutils.secrets.get(scope="prod-secrets", key="db-password")
```

---

### ✅ Summary Checklist for Stage 8:

* [ ] Set up Unity Catalog for data access control
* [ ] Integrate notebooks with Git
* [ ] Build CI/CD pipelines using Databricks CLI or Terraform
* [ ] Monitor and debug job/cluster logs
* [ ] Securely manage credentials using Secrets

---

### 📘 Real-World Analogy:

> Think of this stage like managing a **well-governed city**: roads (data pipelines), speed limits (access control), police (audit logs), repair teams (CI/CD), and secure vaults (secrets management).

---


