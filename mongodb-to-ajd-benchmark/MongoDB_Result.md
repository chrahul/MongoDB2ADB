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
---
---


 **Workload B (Read-Heavy)** benchmark run on **MongoDB (16M records)**:



###  **Test Parameters**

* **Database**: MongoDB (local)
* **Record Count**: 16 million
* **Operations**: 600,000
* **Workload**: `workloadb` (95% Reads, 5% Updates)
* **Threads**: 16
* **YCSB Command**:

  ```bash
  ./bin/ycsb run mongodb -s -P workloads/workloadb \
    -p recordcount=16000000 \
    -p operationcount=600000 \
    -p threadcount=16 \
    -p mongodb.url="mongodb://localhost:27017/ycsb?w=1"
  ```

---

###  **Performance Metrics**

####  Overall

* **Total RunTime**: `41.7 seconds`
* **Throughput**: `14,375 ops/sec`

####  Read Operations (570,080 reads)

* **Avg Latency**: `1091.67 µs`
* **Min Latency**: `122 µs`
* **Max Latency**: `169,087 µs`
* **50th Percentile (Median)**: `885 µs`
* **95th Percentile**: `2,469 µs`
* **99th Percentile**: `4,343 µs`

####  Update Operations (29,920 updates)

* **Avg Latency**: `1231.01 µs`
* **Min Latency**: `171 µs`
* **Max Latency**: `113,983 µs`
* **50th Percentile**: `987 µs`
* **95th Percentile**: `2,749 µs`
* **99th Percentile**: `4,891 µs`

####  Garbage Collection (GC)

* **Young Gen GC Count**: 94
* **Young Gen GC Time**: `137 ms`
* **GC Overhead**: \~`0.33%` of total runtime

---

###  Observations

* The MongoDB server handled the **read-heavy workload efficiently**, sustaining **14K+ ops/sec throughput** with low GC overhead.
* Latency is **well within acceptable limits**, especially the 50th and 95th percentiles.
* **Read operations dominate the workload**, and the system responded with consistently low latencies, making MongoDB suitable for read-heavy use cases at this scale.

---
---
---


**MongoDB Workload C (Update-Heavy, 16M records)** benchmark run:



###  **Benchmark Summary (Workload C – 16M records)**

**Command Run:**

```
./bin/ycsb run mongodb -s -P workloads/workloadc \
  -p recordcount=16000000 \
  -p operationcount=600000 \
  -p threadcount=16 \
  -p mongodb.url="mongodb://localhost:27017/ycsb?w=1" \
  > output-mongo-run-16m-workloadc.txt
```

---

###  **Performance Metrics**

| Metric                   | Value                    |
| ------------------------ | ------------------------ |
| **Total Operations**     | 600,000                  |
| **Run Time**             | 40.9 sec                 |
| **Throughput**           | **14,667 ops/sec**       |
| **Avg Latency (Update)** | **1,075 µs** (≈ 1.07 ms) |
| **Min Latency**          | 122 µs                   |
| **Max Latency**          | 67,135 µs                |
| **50th Percentile**      | 872 µs                   |
| **95th Percentile**      | 2,487 µs                 |
| **99th Percentile**      | 4,355 µs                 |

---

###  **Cleanup Performance**

| Metric                  | Value    |
| ----------------------- | -------- |
| **Cleanup Ops**         | 16       |
| **Avg Cleanup Latency** | 372 µs   |
| **Max Cleanup Latency** | 5,939 µs |

---

###  **Garbage Collection**

| GC Type               | Count | Time (ms)                     | Time % |
| --------------------- | ----- | ----------------------------- | ------ |
| **Young Gen (G1)**    | 96    | 133 ms                        | 0.33%  |
| **Old/Concurrent GC** | 0     | 0 ms                          | 0.00%  |
| **Total GC Events**   | 96    | Total GC Time: 133 ms (0.33%) |        |

---

###  Observations

* **Consistent throughput** of \~14.6K ops/sec even under update-heavy conditions.
* Update latency remained within expected bounds — with 95% updates completing in <2.5 ms.
* Garbage collection overhead is negligible (<0.5%), indicating memory handling is optimal.

---

 **Conclusion**: MongoDB performed **robustly under Write-Heavy (Update) workload** at the 16M scale. Throughput held steady and latencies were low, confirming the system's efficiency for OLTP-like write patterns.

---




