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

