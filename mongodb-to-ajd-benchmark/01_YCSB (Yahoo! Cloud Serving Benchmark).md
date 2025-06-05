**YCSB (Yahoo! Cloud Serving Benchmark)** 

---

## What is **YCSB**?

**YCSB** stands for **Yahoo! Cloud Serving Benchmark**.
It is an **open-source benchmarking framework** created by Yahoo! to **evaluate and compare the performance of modern NoSQL databases** and **cloud data services**.

---

### **Purpose of YCSB**

YCSB is designed to **simulate real-world workloads** on data stores and measure:

* **Latency (response time)**
* **Throughput (operations per second)**
* **Performance under load**
* **Scalability across clusters or cloud setups**

---

### **What YCSB Can Benchmark**

YCSB supports several NoSQL and cloud-native databases out of the box:

| Database               | Supported? |
| ---------------------- | ---------- |
| **MongoDB**            | Yes      |
| **Cassandra**          | Yes      |
| **DynamoDB**           | Yes      |
| **HBase**              | Yes      |
| **Couchbase**          | Yes      |
| **Redis (via plugin)** | Yes      |

And you can extend it to benchmark **Oracle AJD (via MongoDB API)** by configuring it like a MongoDB target.

---

### **How It Works (High Level)**

1. **Load Phase** – Insert a large number of records (e.g., 1 million documents).
2. **Run Phase** – Simulate real-world operations like:

   * Reads
   * Updates
   * Inserts
   * Scans (range queries)
   * Deletes

You can configure the **mix** of these operations using **workload files** (e.g., Workload A = 50% reads, 50% writes).

---

### **Sample Use Cases**

| Use Case                        | How YCSB Helps                                   |
| ------------------------------- | ------------------------------------------------ |
| Benchmark MongoDB vs Oracle AJD | Same workload against both using Mongo API       |
| Compare read/write latency      | Measures average, p95, p99 latencies             |
| Simulate app behavior at scale  | Run 10k+ operations per second to test stability |
| Evaluate performance under load | Run multi-threaded tests for concurrency         |

---

### **Common Predefined Workloads**

| Workload | Read % | Update %   | Use Case                          |
| -------- | ------ | ---------- | --------------------------------- |
| A        | 50     | 50         | Balanced read/write               |
| B        | 95     | 5          | Read-heavy (e.g., user profile)   |
| C        | 100    | 0          | Read-only (e.g., cache lookup)    |
| D        | 95     | 5 (insert) | Read + insert (e.g., log entries) |
| E        | 95     | 5 (scan)   | Read + short range scans          |
| F        | 50     | 50         | Read/modify/write                 |

---

### **Typical Output**

After you run YCSB, it reports:

```txt
[READ], AverageLatency(us), 520
[READ], 95thPercentileLatency(us), 1100
[UPDATE], AverageLatency(us), 750
[OVERALL], Throughput(ops/sec), 1325
```

This gives a **quantitative benchmark** of performance under a controlled workload — great for comparing MongoDB vs AJD side-by-side.

---


