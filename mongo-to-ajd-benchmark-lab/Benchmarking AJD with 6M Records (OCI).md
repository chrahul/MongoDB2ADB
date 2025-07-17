**Step-by-Step Guide for Lab #3: Benchmarking Oracle Autonomous JSON Database (AJD) with 6 Million Records** using **YCSB**.

---

## Lab #3: Benchmarking AJD with 6M Records (OCI)

### Environment Setup Recap

| Component             | Configuration                            |
| --------------------- | ---------------------------------------- |
| YCSB Version          | Latest (reinstalled)                     |
| Target DB             | Oracle Autonomous JSON Database (AJD)    |
| VM OS                 | Oracle Linux / CentOS on OCI             |
| Records to Load       | `6,000,000` (`recordcount=6000000`)      |
| Threads               | `10` (or adjust for performance testing) |
| Workloads             | A, B, C, D, E, F                         |
| Oracle JDBC Driver    | Already installed (`ojdbc8.jar`)         |
| AJD Connection String | Same as Lab #2 (validated)               |

---

### Step-by-Step Execution for Each Workload

> Replace `workloadX` with `workloada`, `workloadb`, etc. Run **each workload one by one**.

---

#### Step 1: Load 6 Million Records

```bash
./bin/ycsb load jdbc \
  -s \
  -P workloads/workloadX \
  -cp jdbc-binding/conf \
  -p recordcount=6000000 \
  -p operationcount=6000000 \
  -p jdbc.driver=oracle.jdbc.OracleDriver \
  -p db.url=jdbc:oracle:thin:@<AJD_CONNECTION_STRING> \
  -p db.user=<USERNAME> \
  -p db.passwd=<PASSWORD> \
  -p jdbc.fetchsize=1000 \
  -p jdbc.batchsize=500 \
  -p insertorder=hashed \
  > output-load-6m-workloadX.txt
```

---

#### Step 2: Run the Benchmark Workload

```bash
./bin/ycsb run jdbc \
  -s \
  -P workloads/workloadX \
  -cp jdbc-binding/conf \
  -p recordcount=6000000 \
  -p operationcount=6000000 \
  -p jdbc.driver=oracle.jdbc.OracleDriver \
  -p db.url=jdbc:oracle:thin:@<AJD_CONNECTION_STRING> \
  -p db.user=<USERNAME> \
  -p db.passwd=<PASSWORD> \
  -p jdbc.fetchsize=1000 \
  -p jdbc.batchsize=500 \
  -threads 10 \
  > output-run-6m-workloadX.txt
```

---

### Step 3: Capture and Archive Results

For each workload (A–F), you will have two files:

* `output-load-6m-workloadX.txt`
* `output-run-6m-workloadX.txt`

Save and zip all logs.

---

### Notes & Tips

* Start with **Workload A** and scale sequentially to F.
* Monitor memory/CPU on the VM while running.
* If any run fails, retry with `threads=5` or `operationcount=3000000`.
* We’ll later extract: throughput (ops/sec), read/write latency, 95th percentile.

---


---

## Lab #3 – YCSB Benchmark Result Capture Template (AJD)

| Workload | Operation Count | Threads | Throughput (ops/sec) | Read Latency (Avg, 95th) | Write Latency (Avg, 95th) | Update Latency (Avg, 95th) | Scan Latency (Avg, 95th) | Insert Latency (Avg, 95th) | Notes |
| -------- | --------------- | ------- | -------------------- | ------------------------ | ------------------------- | -------------------------- | ------------------------ | -------------------------- | ----- |
| A        | 6,000,000       | 10      |                      |                          |                           |                            |                          |                            |       |
| B        | 6,000,000       | 10      |                      |                          |                           |                            |                          |                            |       |
| C        | 6,000,000       | 10      |                      |                          |                           |                            |                          |                            |       |
| D        | 6,000,000       | 10      |                      |                          |                           |                            |                          |                            |       |
| E        | 6,000,000       | 10      |                      |                          |                           |                            |                          |                            |       |
| F        | 6,000,000       | 10      |                      |                          |                           |                            |                          |                            |       |

---

### Column Explanation

| Column Name               | Meaning                                               |
| ------------------------- | ----------------------------------------------------- |
| **Workload**              | YCSB workload name (A–F)                              |
| **Operation Count**       | Total operations performed (6M)                       |
| **Threads**               | Number of concurrent threads used (usually 10)        |
| **Throughput**            | Operations per second achieved                        |
| **Read/Write/Update/...** | Latency (avg + 95th percentile) in milliseconds       |
| **Notes**                 | Any error, retry, or anomaly to remember for that run |

---



