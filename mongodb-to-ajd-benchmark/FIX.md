
---

##  Root Cause Analysis: Why Your Test Is Too Fast

Despite configuring `recordcount=1000000` and `operationcount=1000000`, the **actual load** done by YCSB appears **very minimal**. Here's **why** this could happen:

---

###  1. **Properties File Location**

You're using:

```bash
-P jdbc/oracle-ajd.properties \
-P workloads/workloada
```

But **if the `workloada` file overrides** values from `oracle-ajd.properties` (which it often does), your recordcount/operationcount may be **reset to default (1000)**.

 **Fix:**

* Open `workloada` (under `workloads/`) and **explicitly set**:

```
recordcount=1000000
operationcount=1000000
```

* Or add a custom workload file like `workload_large` with these settings.

---

###  2. **Missing `threads` Argument**

By default, YCSB uses a **single thread**, so even with 1M operations, it will take a long time. If you forget to **increase threads**, the test will run on just 1 core.

🔧 **Fix:**
Use `-threads 20` (or any value suitable for your VM’s CPU). For example:

```bash
./jdbc/target/ycsb-jdbc-binding-*/bin/ycsb load jdbc -s \
  -P jdbc/oracle-ajd.properties \
  -P workloads/workload_large \
  -threads 20
```

---

###  3. **Pre-populated Data?**

Check if this was the **second run** on the same table — i.e., data was already inserted earlier. If yes, YCSB might **skip** inserts or throw duplicate errors silently depending on the workload logic.

 **Fix:**
Drop the table before starting a new run:

```sql
DROP TABLE usertable;
```

Then re-run the load phase.

---

##  Additional Suggestions

### 4. **Use Log File to Verify Records Inserted**

Always append `-p status.interval=1` to get per-second logs.

Also, capture full output using:

```bash
... > load.log 2>&1
```

Search in the log for:

```bash
[INSERT], Operations
```

If it shows **< 1000 inserts**, your `operationcount` didn’t apply.

---

##  Sample Working Command for Large Test

```bash
./jdbc/target/ycsb-jdbc-binding-*/bin/ycsb load jdbc -s \
  -P jdbc/oracle-ajd.properties \
  -P workloads/workload_large \
  -threads 20 \
  -p status.interval=1
```

Inside `workload_large` file:

```
recordcount=1000000
operationcount=1000000
readproportion=0.5
updateproportion=0.5
insertproportion=0
```

---

##  How Oracle Did It (from their PDF)

In Oracle’s public test:

* They ran **10M records** and **1M ops**
* On **multi-vCPU environments** using **multi-threaded** runs
* Likely tuned `batchupdateapi` and connection pooling as well

---

##  Action Plan for You

| Step | Fix / Check                                             |
| ---- | ------------------------------------------------------- |
| 1️  | Create a new custom workload file like `workload_large` |
| 2️  | Set both `recordcount` and `operationcount` to 1M       |
| 3️  | Pass `-threads 20` to simulate concurrency              |
| 4️  | Clean DB before every run (`DROP TABLE usertable;`)     |
| 5️  | Capture logs and verify real insert count               |

---


