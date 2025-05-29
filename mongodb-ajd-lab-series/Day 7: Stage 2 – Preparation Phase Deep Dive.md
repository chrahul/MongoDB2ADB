
---

# **Day 7: Stage 2 – Preparation Phase Deep Dive**


![image](https://github.com/user-attachments/assets/aad75e10-bc7e-4535-8fe4-8a479dbbe5ce)


---

## Objective:

Understand everything needed to **prepare the Oracle target environment**, ensure **compatibility**, and select the **right migration tools**.

---

## Overview of the Preparation Phase

The Preparation phase is where you **get your hands dirty** — spinning up the Oracle AJD environment, selecting tooling, and preparing migration strategies.

---

## 1. **Target System Setup**

### Supported Target Systems:

* **Oracle Database 21c or later**
* **Oracle Autonomous Database (ADB)**:

  * Serverless
  * Dedicated
  * Cloud\@Customer
* **Oracle Database 23ai** → *preferred for best Mongo compatibility*

### Required Components:

* **Oracle REST Data Services (ORDS)** 22.3 or later
  Needed if not using Autonomous DB

---

## 2. **Target Sizing Considerations**

Before provisioning AJD:

* Consider **MongoDB workload patterns**
* Estimate:

  * **Data size**
  * **CPU cores**
  * **Storage**
  * **Concurrency**

*Note: AJD offers auto-scaling. You can start small and grow as needed.*

---

##  3. **Pre-Migration Backup**

Always create a backup before any data move:

### Use `mongodump`:

```bash
mongodump --archive=backup.archive --oplog
```

* `--archive` → Saves to a binary file
* `--oplog` → Captures in-flight writes for consistency

---

## 4. **Prepare Test & Rollback Plans**

| Task               | Description                                        |
| -------------------- | -------------------------------------------------- |
| **Test Cases**       | Functional, Integration, System tests              |
| **Rollback Plan**    | Document steps to revert to MongoDB if needed      |
| **Monitoring Tools** | Setup logs, alerts, metrics via OCI or third-party |

---

## 5. **Migration Infrastructure Checklist**

| Component            | Purpose                                      |
| -------------------- | -------------------------------------------- |
| VM or Cloud Instance | Hosting transformation scripts, staging data |
| Object Storage       | Upload exported JSON/flat files              |
| VCN, Routes          | Enable communication between app ↔ AJD      |
| IAM Roles            | Controlled access                            |
| Encryption Tools     | Secure data at rest/in transit               |
| Terraform/Ansible    | Automate deployment & configs                |

---

##  6. **Migration Tool Options**

Choose based on your team’s strengths:

| Source                 | Oracle Tool                            | Best For                     |
| ---------------------- | -------------------------------------- | ---------------------------- |
| MongoDB Dump           | `mongoexport` + Oracle External Tables | Simplicity                   |
| JSON to Object Storage | `DBMS_CLOUD`                           | OCI-native bulk loading      |
| Live Sync              | Oracle GoldenGate (OGG)                | Near-zero downtime migration |
| JSON Flat Files        | External Tables                        | Structured ingest            |
| Custom Pipelines       | Python/Node.js scripts                 | Hybrid approaches            |

*Tool selection depends on how live or real-time your cutover needs to be.*

---

## Example: External Table Load via JSON

```sql
CREATE TABLE ext_json_data (
  doc CLOB
)
ORGANIZATION EXTERNAL
(
  TYPE ORACLE_LOADER
  DEFAULT DIRECTORY ext_dir
  ACCESS PARAMETERS (
    RECORDS DELIMITED BY NEWLINE
    FIELDS (doc CHAR(1000000))
  )
  LOCATION ('mongo_export.json')
)
```

---

## Suggested Output: `docs/day7_preparation_phase.md`

Break it down into:

* Target system details
* Sizing estimate
* Tools selected
* Backup command
* Rollback steps
* Migration method selected
* Test plans

---

## Day 7 Tasks

| Task        | Description                                                               | Done? |
| ----------- | ------------------------------------------------------------------------- | ----- |
| Read     | Pages 8–10 of Oracle guide                                                |      |
| Write    | `docs/day7_preparation_phase.md`                                          |      |
| Backup   | Practice `mongodump` locally if possible                                  |      |
| Tool Pick | Decide which tool(s) you’ll use in your mock migration                    |      |
| Optional | Want a diagram showing different migration paths (OGG, JSON, Ext Tables)? |      |

---

