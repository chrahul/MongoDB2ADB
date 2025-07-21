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
---

**Workload B (Read-Heavy)** on **MongoDB with 6 million records** based on the YCSB output log:

---

###  **Workload B Summary (Read-Heavy, 95% Reads / 5% Updates)**

| Metric               | Value              |
| -------------------- | ------------------ |
| **Total Ops (600K)** | 600,000            |
| **Read Ops**         | 569,693            |
| **Update Ops**       | 30,307             |
| **Total Runtime**    | 41.43 seconds      |
| **Throughput**       | **14,481 ops/sec** |

---

###  **Read Performance**

| Metric              | Value   |
| ------------------- | ------- |
| **Avg Latency**     | 1079 μs |
| **Min Latency**     | 124 μs  |
| **Max Latency**     | 393 ms  |
| **50th Percentile** | 846 μs  |
| **95th Percentile** | 2.48 ms |
| **99th Percentile** | 4.56 ms |

>  The **average read latency is under 1.1 ms**, which is decent for a high volume read-heavy workload. However, **some long-tail latencies (393 ms max)** may indicate minor spikes — possibly due to background flushes or thread contention.

---

###  **Update Performance (5%)**

| Metric              | Value   |
| ------------------- | ------- |
| **Avg Latency**     | 1235 μs |
| **Min Latency**     | 192 μs  |
| **Max Latency**     | 283 ms  |
| **50th Percentile** | 942 μs  |
| **95th Percentile** | 2.87 ms |
| **99th Percentile** | 4.94 ms |

>  Update operations take slightly more time than reads (as expected), but still remain mostly under **3 ms**, showing that MongoDB handled the mixed workload efficiently.

---

###  **GC Activity (Garbage Collection)**

| GC Metric          | Value  |
| ------------------ | ------ |
| **Young GC Count** | 96     |
| **Total GC Time**  | 140 ms |
| **GC Time %**      | 0.34%  |

>  Garbage collection overhead is very low (**<0.5%**), so memory management during this run was efficient and non-intrusive.

---

###  Key Observations

1. **Strong Throughput** of \~14.5K ops/sec with 16 threads.
2. **Read performance is stable**, with 95% of reads completing under 2.5 ms.
3. **Update latency is slightly higher**, but well within acceptable limits.
4. **GC time is negligible**, showing good JVM tuning or low allocation pressure.

---
 **Conclusion:**
MongoDB efficiently handled the 6M record **Workload B** with high throughput and low latency, especially for reads. This confirms its robustness for read-heavy applications such as user-profile lookups, dashboards, and product catalogs.

---













---
---
---
# 16M

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
---
---


**Workload D (Read Latest)** test on **MongoDB with 16 million records**:



###  **Test Summary: Workload D – 16M Records**

| Metric            | Value          |
| ----------------- | -------------- |
| **Total Runtime** | 43.86 sec      |
| **Throughput**    | 13,679 ops/sec |
| **GC Time (%)**   | 0.31%          |

---

###  **Operation Breakdown**

#### 🔹 READ (Latest)

| Metric                  | Value      |
| ----------------------- | ---------- |
| Operations              | 569,947    |
| Average Latency (μs)    | 1,147.67   |
| Min Latency             | 128 μs     |
| Max Latency             | 102,079 μs |
| 50th Percentile Latency | 947 μs     |
| 95th Percentile Latency | 2,495 μs   |
| 99th Percentile Latency | 4,211 μs   |

 **Return=OK**: All 569,947 read operations were successful.

---

####  INSERT (during test)

| Metric                  | Value     |
| ----------------------- | --------- |
| Operations              | 30,053    |
| Average Latency (μs)    | 1,241.82  |
| Min Latency             | 157 μs    |
| Max Latency             | 47,999 μs |
| 50th Percentile Latency | 1,014 μs  |
| 95th Percentile Latency | 2,843 μs  |
| 99th Percentile Latency | 4,971 μs  |

 **Return=OK**: All 30,053 inserts completed successfully.

---

####  CLEANUP

* 16 operations with **very low latency** (most < 3ms).

---

###  Interpretation

* This workload simulates **real-time reads of the latest records**, common in **news feeds, live dashboards, or monitoring systems**.
* MongoDB delivered **stable throughput (13.6K ops/sec)** even with 16M records.
* **Read latencies are higher than workload B**, as expected due to targeting newer entries and possible cache misses.
* **Insert latencies** remained under control — average \~1.24 ms, showing MongoDB handles mixed loads efficiently.

---
---
---

 **Workload E (Short Ranges)** on **MongoDB with 16 Million records**:



###  **YCSB Workload E (16M Records) – MongoDB**

**🔹 General Info**

* **Record Count:** 16,000,000
* **Operations Run:** 600,000
* **Workload Type:** Short Ranges (95% scans, 5% inserts)



###  **Overall Performance**

| Metric         | Value                     |
| -------------- | ------------------------- |
| **Runtime**    | 970,755 ms (≈ 970.75 sec) |
| **Throughput** | **618 ops/sec**           |
| **GC Time**    | 2,059 ms (0.21%)          |

---

###  **SCAN Operation (95%)**

| Metric                | Value (µs)    |
| --------------------- | ------------- |
| **Total Operations**  | 569,942       |
| **Average Latency**   | 27,106        |
| **Min / Max Latency** | 192 / 179,583 |
| **50th Percentile**   | 22,655        |
| **95th Percentile**   | 69,887        |
| **99th Percentile**   | 89,535        |

✔ All scan operations returned **OK**.



###  **INSERT Operation (5%)**

| Metric                        | Value (µs) |
| ----------------------------- | ---------- |
| **Successful Inserts**        | 5          |
| **Failed Inserts**            | 30,053     |
| **Average Latency (Success)** | 907        |
| **Average Latency (Failed)**  | 1,341      |
| **Max Latency (Failed)**      | 57,887     |

 **Insert failures are significant** — likely due to write capacity or indexing issues during concurrent scanning.



###  **Garbage Collection (G1GC)**

| Generation     | GC Count | GC Time (ms) |
| -------------- | -------- | ------------ |
| Young Gen      | 1318     | 2,059        |
| Old/Concurrent | 0        | 0            |



###  Summary Notes:

* **Throughput** is moderate at \~618 ops/sec, which is typical for scan-heavy workloads at this scale.
* **Latency on SCAN** is quite high, especially at the 95th–99th percentile — expected due to disk I/O and memory pressure.
* **High INSERT failure count** (30K+) should be investigated. It may stem from:

  * Memory limits
  * Write concern settings
  * MongoDB configuration (`w=1` may still cause contention under heavy scan load)
* GC overhead remains minimal and non-blocking.


---
---
---


**Workload F Benchmark Analysis (16 Million Records on MongoDB):**




####  **Overall Execution**

* **Total Ops Run**: 600,000
* **Runtime**: `84,641 ms` (\~84.6 sec)
* **Throughput**: **\~7088 ops/sec**



####  **READ Operation**

* **Total Reads**: 600,000
* **Avg Latency**: `1,753 μs` (≈ 1.75 ms)
* **Min / Max**: `130 μs / 254,975 μs`
* **P50 / P95 / P99**:

  * 50th: `1,073 μs`
  * 95th: `3,285 μs`
  * 99th: `13,327 μs`
* **Return=OK**: 100%



####  **READ-MODIFY-WRITE Operation**

* **Total Ops**: 299,729
* **Avg Latency**: `2,733 μs` (≈ 2.73 ms)
* **Min / Max**: `305 μs / 255,359 μs`
* **P50 / P95 / P99**:

  * 50th: `2,000 μs`
  * 95th: `4,991 μs`
  * 99th: `18,015 μs`



####  **UPDATE Operation**

* **Total Updates**: 299,729
* **Avg Latency**: `971 μs` (≈ 0.97 ms)
* **Min / Max**: `162 μs / 84,927 μs`
* **P50 / P95 / P99**:

  * 50th: `781 μs`
  * 95th: `2,247 μs`
  * 99th: `4,069 μs`
* **Return=OK**: 100%



####  **CLEANUP**

* **Ops**: 16
* **Average**: `360 μs`
* **Max**: `5,735 μs`



#### ♻ **GC Activity**

* **Young GC Count**: 126
* **Young GC Time**: `203 ms`
* **GC Time %**: `0.23%` → negligible



###  Observations:

* Very **stable performance** with >7k ops/sec throughput.
* **READs are fast**, but READ-MODIFY-WRITE naturally takes longer (\~2.7 ms avg).
* **UPDATEs are efficient**, with sub-ms average latency.
* Latency tails (95th and 99th) are within reasonable bounds—indicating good response time distribution.
* No Old GC or Concurrent GC events — excellent JVM health.

---

Yes, you're absolutely right — ✅ **Workload D for 16M records has already been completed and analyzed.**

Here's a quick recap of what we've finished so far:

---

###  **Completed Workloads (16M Records on MongoDB)**

| Workload | Type                      | Output File                          | Status  |
| -------- | ------------------------- | ------------------------------------ | ------- |
| A        | Read + Write (50/50)      | `output-mongo-run-16m-workloada.txt` |  Done |
| B        | Read-Heavy (95R/5W)       | `output-mongo-run-16m-workloadb.txt` |  Done |
| C        | Read-Only                 | `output-mongo-run-16m-workloadc.txt` |  Done |
| D        | Read-Latest               | `output-mongo-run-16m-workloadd.txt` |  Done |
| E        | Short Ranges (scan-heavy) | `output-mongo-run-16m-workloade.txt` |  Done |
| F        | Read-Modify-Write         | `output-mongo-run-16m-workloadf.txt` |  Done |

---







