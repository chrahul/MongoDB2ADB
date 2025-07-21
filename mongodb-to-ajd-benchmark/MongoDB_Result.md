**Workload A with 6 million records on MongoDB** 

---

###  **Benchmark Summary (YCSB Workload A – 6M records, Run Phase):**

* **Throughput (Ops/sec):** `6312.28`
* **Run time:** `99998 ms` (approx. 100 seconds)
* **Operations performed:** `630837` (close to the target of 600k+)
* **Latency metrics:**

  * **Average Read Latency:** `12.32 ms`
  * **Average Update Latency:** `17.51 ms`
  * **95th Percentile Latency:** `30 ms`
  * **99th Percentile Latency:** `53 ms`
* **Errors:** `0` (no failed operations)

---

### **Conclusion:**

* You’ve **successfully loaded and benchmarked** 6 million records using **Workload A** on MongoDB.
* The results are **valid** and consistent with expected behavior for a mixed workload (50% reads, 50% updates).
* Latency is within an acceptable range for such a dataset size — especially useful for your MongoDB vs AJD performance comparison.

---
---

**summary of findings** from your **MongoDB YCSB Load Test for 16 Million Records (Workload A)** 

---

### **Test Details**

* **Database**: MongoDB
* **YCSB Workload**: A (50% reads, 50% updates)
* **Operation Type**: Load (Inserts only)
* **Record Count**: 16,000,000
* **Thread Count**: Not explicitly mentioned in output (assumed default = 1 or previously set)

---

### **Performance Metrics (Load Phase)**

| Metric                          | Value               |
| ------------------------------- | ------------------- |
| **Total Operations Attempted**  | 16,000,000          |
| **Successful Inserts**          | 15,999,983          |
| **Insert Failures**             | 17                  |
| **Average Insert Latency**      | **\~3.29 ms**       |
| **Insert Throughput (ops/sec)** | **3039.56 ops/sec** |

---

### **Insert Latency Distribution (in microseconds)**

| Percentile      | Latency (µs) |
| --------------- | ------------ |
| Min             | 440          |
| Max             | 39,551       |
| 50th percentile | 3,248        |
| 90th percentile | 5,359        |
| 99th percentile | 8,623        |
| 99.9th          | 12,863       |
| 99.99th         | 18,335       |

> Overall, the latencies are consistent and indicate that MongoDB handled the 16M insert load efficiently under the current system configuration.

---

### **Insert Failures Summary**

* **Count**: 17
* **Reason**: Duplicate key errors on `_id` field (`E11000 duplicate key error`)
* **Cause**: Likely due to:

  * Partial previous inserts not cleaned properly
  * Parallelism-related UUID/ID collision
* **Resolution**: Already handled — you dropped the DB using `mongosh` before retrying, which is correct.

---

### Observations

1. **Compared to the 6M test**, the average insert latency is nearly identical (\~3.29 ms vs \~3.25 ms), showing **good scalability**.
2. Throughput of \~3000 ops/sec is consistent and **linear with scale**, which is a **positive indicator of stability**.
3. The 17 failures are negligible (<0.0001%) and expected with massive insert workloads unless bulk-safe mechanisms or deterministic IDs are enforced.

---
---

**YCSB Workload A (Run Phase)** for **16 million records on MongoDB** 

Here are the **key performance findings** from the output:

---

###  **Workload A (Run Phase – 16M records on MongoDB)**

| Metric               | Value                   |
| -------------------- | ----------------------- |
| **Operation Count**  | `600000` ops            |
| **Threads Used**     | `16`                    |
| **Throughput**       | **2601.84 ops/sec**     |
| **Test Duration**    | \~230.6 seconds         |
| **Workload Pattern** | 50% Reads / 50% Updates |

---

###  **Latency Analysis**

#### **Read Operations (50%)**

| Percentile | Latency (in microseconds) |
| ---------- | ------------------------- |
| Average    | **5850 µs** *(\~5.85 ms)* |
| 50th       | 4575 µs                   |
| 90th       | 10575 µs                  |
| 99th       | 13791 µs                  |
| 99.9th     | 18239 µs                  |

#### **Update Operations (50%)**

| Percentile | Latency (in microseconds) |
| ---------- | ------------------------- |
| Average    | **5227 µs** *(\~5.2 ms)*  |
| 50th       | 4303 µs                   |
| 90th       | 9567 µs                   |
| 99th       | 12095 µs                  |
| 99.9th     | 16447 µs                  |

---

###  **Interpretation**

* **Throughput (\~2600 ops/sec)** is decent, given the scale (16M records) and the hardware.
* **Latency remains within acceptable bounds**, showing that MongoDB handles mixed read/write workloads efficiently even at scale.
* **Tail latencies** (99.9th percentile) stay under \~18 ms, which is good for large-scale applications with moderate real-time needs.

---

###  Summary for Report

> For a 16M document dataset on MongoDB (Workload A – 50/50 read/write), the database achieved **\~2600 ops/sec throughput** with **median latencies under 5 ms**, and **99.9th percentile below 18 ms**. This confirms MongoDB’s ability to handle medium-to-high transactional workloads at scale with consistent performance.

---

