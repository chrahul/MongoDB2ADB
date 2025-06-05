List of widely-used and effective **NoSQL benchmarking tools** we can use:

---

### **Top NoSQL Benchmarking Tools**

#### 1. **YCSB (Yahoo! Cloud Serving Benchmark)**

* **Most popular benchmarking tool for NoSQL**
* Supports: **MongoDB, Cassandra, DynamoDB, HBase, Couchbase**, etc.
* Measures: **Throughput, latency, read/write mix**
* Highly customizable workloads (A-F patterns)

**Use case**: Run side-by-side benchmarks of MongoDB and AJD (via Mongo API) using the same dataset and workload.

GitHub: [https://github.com/brianfrankcooper/YCSB](https://github.com/brianfrankcooper/YCSB)

---

#### 2. **mongo-perf**

* Developed by MongoDB Inc.
* Designed to measure **low-level performance** of individual operations
* Can test: `insert`, `update`, `find`, `aggregate`, etc.

**Use case**: Benchmark the raw performance of MongoDB driver operations and compare it to AJD (Mongo API).

GitHub: [https://github.com/mongodb/mongo-perf](https://github.com/mongodb/mongo-perf)

---

#### 3. **JMeter with MongoDB Plugin**

* Apache JMeter is usually used for API and load testing
* Has a plugin for MongoDB operations
* Allows complex workflows and app-level performance testing

**Use case**: Test MongoDB workloads at scale (100s of concurrent users) and simulate real app behavior.

Plugin: [https://jmeter-plugins.org/wiki/MongoDB-Source/](https://jmeter-plugins.org/wiki/MongoDB-Source/)

---

#### 4. **wrk / wrk2**

* High-performance HTTP benchmarking tool
* Can be used to benchmark REST APIs that indirectly hit MongoDB/AJD (e.g., through a Node.js app)

**Use case**: Benchmark actual app-layer performance while hitting Mongo or AJD endpoints.

GitHub: [https://github.com/wg/wrk](https://github.com/wg/wrk)

---

#### 5. **NoSQLBench**

* A modern, extensible benchmark platform (originally for Cassandra)
* Supports scripting workloads
* Can be extended to MongoDB and other NoSQL databases

Site: [https://nosqlbench.io/](https://nosqlbench.io/)

---

#### 6. **Custom Scripts using mongosh / pymongo**

* Write your own benchmarking scripts using:

  * `mongosh` (JavaScript shell)
  * `pymongo` (Python driver)
  * `node-mongodb-native` (Node.js driver)

**Use case**: Perfect for comparing **app-level latencies** with MongoDB vs Oracle AJD using MongoDB API.

---

### Metrics to Capture During Benchmarking

| Category         | Key Metrics                            |
| ---------------- | -------------------------------------- |
| **Latency**      | Avg, p95, p99 for each op (read/write) |
| **Throughput**   | Ops/sec under various loads            |
| **Concurrency**  | Performance under 50, 100, 500 users   |
| **Resource Use** | CPU, memory, IOPS                      |
| **Error Rate**   | Timeout %, failed ops, retries         |

---

### Recommendation for You

If you want to keep it practical and demo-ready:

| Use Case                   | Tool Recommendation           |
| -------------------------- | ----------------------------- |
| Raw MongoDB vs AJD ops     | **mongo-perf** or **YCSB**    |
| Simulating real users      | **JMeter** + app              |
| REST API / App performance | **wrk** or **Postman Runner** |
| Custom scenario testing    | **pymongo / mongosh** scripts |

---

