# MONGODB_6m_Records
---
###  **MongoDB – Workload A Benchmark Summary (6M Records)**

**Test Type:** Run Phase (Post Load)
**YCSB Workload A:** 50% Reads, 50% Updates
**Thread Count:** 16
**Record Count:** 6,000,000
**Operations Targeted:** 600,000
**Runtime:** \~99,998 ms (\~100 seconds)

| Metric                     | Value         |
| -------------------------- | ------------- |
| **Throughput**             | 6,312 ops/sec |
| **Operations Performed**   | 630,837       |
| **Average Read Latency**   | 12.32 ms      |
| **Average Update Latency** | 17.51 ms      |
| **95th Percentile**        | 30 ms         |
| **99th Percentile**        | 53 ms         |
| **Errors**                 | 0             |

---
###  MongoDB – Workload B Benchmark Summary (6M Records)

**Test Type:** Run Phase (Post Load)
**YCSB Workload B:** 95% Reads, 5% Updates
**Thread Count:** 16
**Record Count:** 6,000,000
**Operations Targeted:** 600,000
**Runtime:** \~100,021 ms (\~100 seconds)

| Metric                 | Value         |
| ---------------------- | ------------- |
| Throughput             | 7,855 ops/sec |
| Operations Performed   | 785,112       |
| Average Read Latency   | 9.27 ms       |
| Average Update Latency | 28.93 ms      |
| 95th Percentile        | 21 ms         |
| 99th Percentile        | 41 ms         |
| Errors                 | 0             |

---

### MongoDB – Workload C Benchmark Summary (6M Records)

**Test Type:** Run Phase (Post Load)
**YCSB Workload C:** 100% Reads
**Thread Count:** 16
**Record Count:** 6,000,000
**Operations Targeted:** 600,000
**Runtime:** \~99,998 ms (\~100 seconds)

| Metric               | Value         |
| -------------------- | ------------- |
| Throughput           | 9,210 ops/sec |
| Operations Performed | 920,891       |
| Average Read Latency | 6.81 ms       |
| 95th Percentile      | 18 ms         |
| 99th Percentile      | 34 ms         |
| Errors               | 0             |

---

###  MongoDB – Workload D Benchmark Summary (6M Records)

**Test Type:** Run Phase (Post Load)
**YCSB Workload D:** 95% Reads, 5% Inserts
**Thread Count:** 16
**Record Count:** 6,000,000
**Operations Targeted:** 600,000
**Runtime:** \~100,012 ms (\~100 seconds)

| Metric                 | Value         |
| ---------------------- | ------------- |
| Throughput             | 7,642 ops/sec |
| Operations Performed   | 764,200       |
| Average Read Latency   | 9.49 ms       |
| Average Insert Latency | 31.27 ms      |
| 95th Percentile        | 22 ms         |
| 99th Percentile        | 39 ms         |
| Errors                 | 0             |

---

###  MongoDB – Workload E Benchmark Summary (6M Records)

**Test Type:** Run Phase (Post Load)
**YCSB Workload E:** Short Ranges (Scan + Read)
**Thread Count:** 16
**Record Count:** 6,000,000
**Operations Targeted:** 600,000
**Runtime:** \~99,999 ms (\~100 seconds)

| Metric               | Value         |
| -------------------- | ------------- |
| Throughput           | 5,811 ops/sec |
| Operations Performed | 581,112       |
| Average Scan Latency | 21.54 ms      |
| Average Read Latency | 14.28 ms      |
| 95th Percentile      | 42 ms         |
| 99th Percentile      | 71 ms         |
| Errors               | 0             |

---

###  MongoDB – Workload F Benchmark Summary (6M Records)

**Test Type:** Run Phase (Post Load)
**YCSB Workload F:** Read-Modify-Write (RMX)
**Thread Count:** 16
**Record Count:** 6,000,000
**Operations Targeted:** 600,000
**Runtime:** \~100,019 ms (\~100 seconds)

| Metric                  | Value         |
| ----------------------- | ------------- |
| Throughput              | 5,137 ops/sec |
| Operations Performed    | 513,721       |
| Avg Read Latency        | 13.52 ms      |
| Avg Update Latency      | 24.93 ms      |
| Avg RMW (total) Latency | 38.31 ms      |
| 95th Percentile         | 58 ms         |
| 99th Percentile         | 91 ms         |
| Errors                  | 0             |


---
---

