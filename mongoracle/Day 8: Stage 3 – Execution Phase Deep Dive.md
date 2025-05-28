
---

# Day 8: Stage 3 – Execution Phase Deep Dive

---

## Goal

This phase is where the **real migration happens** — data, collections, indexes, and even parts of your application are moved from MongoDB into Oracle AJD.

---

## Key Components of Execution Phase

---

### 1. Database Object Migration

Objects to migrate from MongoDB:

* Collections (similar to tables)
* Indexes
* Views (if any)
* Triggers (manual recreation in Oracle if needed)

What to do in Oracle:

* Create JSON-enabled tables or collections
* Recreate indexes
* Use JSON data types (CLOB or BLOB) with SODA or SQL interfaces

---

### 2. Application Code Migration

MongoDB applications use drivers and query language that differ from SQL.

But Oracle AJD supports the **MongoDB wire protocol** — so:

* Minimal or zero changes if you continue using MongoDB drivers
* You can switch to SODA or SQL for advanced querying or integration

---

### 3. Data Migration Methods

Choose based on your downtime tolerance and data size:

#### Option 1 – MongoDB On Prem to Oracle via GoldenGate

* Use **Oracle GoldenGate for MongoDB**
* Achieves **zero or minimal downtime**

#### Option 2 – Export JSON and Load to Object Storage

* Export using `mongoexport`
* Upload to Oracle Object Storage
* Load via `DBMS_CLOUD.copy_data`

#### Option 3 – Flat Files to External Tables

* Export JSON to files
* Define Oracle External Table over those files

#### Option 4 – Direct Ingest via SODA or MongoDB API

* Write custom ETL in Python or Node.js
* Insert directly using MongoDB drivers

---

### 4. Pre-Migration Compatibility Check

Oracle provides a tool called:

**MongoDB Compatibility Advisor**

Steps:

1. Enable query logging in MongoDB
   `db.setProfilingLevel(0, -1)`
2. Run application workload
3. Export `mongod.log`
4. Run `advisor.py` script to get compatibility report

The tool shows:

* Aggregation compatibility
* Unsupported features
* Operators that need remediation

---

## ✅ CLI Command Cheat Sheet

### mongodump

```bash
mongodump --archive=backup.archive --oplog
```

### mongoexport

```bash
mongoexport --db=mydb --collection=users --out=users.json
```

### Oracle DBMS\_CLOUD to load JSON

```sql
BEGIN
  DBMS_CLOUD.COPY_DATA(
    table_name => 'MY_JSON_TABLE',
    file_uri_list => 'https://objectstorage.us-phoenix-1.oraclecloud.com/n/namespace/b/bucket/o/users.json',
    format => JSON_OBJECT('type' value 'json')
  );
END;
```

### Oracle GoldenGate (MongoDB to Oracle)

Documentation:
https colon slash slash docs.oracle.com slash en slash middleware slash goldengate

Basic Flow:

* Setup MongoDB extract
* Setup trail file
* Load into Oracle replicat process

---

## Example External Table

```sql
CREATE TABLE ext_json
(
  doc CLOB
)
ORGANIZATION EXTERNAL
(
  TYPE ORACLE_LOADER
  DEFAULT DIRECTORY ext_dir
  ACCESS PARAMETERS
  (
    RECORDS DELIMITED BY NEWLINE
    FIELDS (doc CHAR(1000000))
  )
  LOCATION ('users.json')
);
```

---

## Your Day 8 Output

Create `docs/day8_execution_phase.md` and include:

* Which migration method you will use
* Commands used
* Your `mongoexport` or `mongodump` test
* Pre-migration advisor summary (if tested)

---

## Day 8 Task Tracker

| Task     | Description                          | Done |
| -------- | ------------------------------------ | ---- |
| Read     | Pages 11–12 of Oracle Guide          |      |
| Create   | `day8_execution_phase.md`            |      |
| Practice | `mongodump` and `mongoexport`        |      |
| Capture  | Advisor report (if used)             |      |
| Pick     | One migration method and explain why |      |

---

Let me know when you are done with Day 8. Tomorrow, we will go into:

