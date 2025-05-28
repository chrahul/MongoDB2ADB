**MongoDB → AJD Mastery | Day 1**

**what is AJD**

---

##  **Day 1: Introduction to Oracle Autonomous JSON Database (AJD)**

###  **Objective:**

Gain a strong foundational understanding of:

* What AJD is
* Why Oracle built it
* How it fits into the MongoDB ecosystem

---

###  1. **What is Oracle Autonomous JSON Database (AJD)?**

AJD is a specialized, **fully-managed cloud-native database** on Oracle Cloud Infrastructure (OCI) designed for **JSON document storage and processing**.

####  Key Highlights:

* Optimized for **document-centric workloads**
* Compatible with **MongoDB APIs** via Oracle Database API for MongoDB
* Runs on **Autonomous Database** (scalable, self-tuning, secure)
* Allows both **NoSQL-style JSON document access** and **relational SQL queries**

---

###  2. **How is AJD Different from MongoDB?**

| Feature        | MongoDB                      | AJD                                    |
| -------------- | ---------------------------- | -------------------------------------- |
| Data Model     | BSON documents               | Native JSON storage                    |
| Query Language | MongoDB Query API            | JSON\_PATH, SQL, SODA                  |
| Transactions   | Limited (multi-doc via 4.0+) | Full ACID support                      |
| Indexing       | B-tree, Text, Geospatial     | Functional, JSON-specific, Full-text   |
| Backup/Restore | Ops manager or manual        | Automated + OCI-native                 |
| APIs           | Mongo Shell, Drivers         | Mongo API, REST, SODA, SQL             |
| Management     | Manual or Atlas              | Autonomous (self-tuning, auto-scaling) |



![image](https://github.com/user-attachments/assets/c8626806-86de-4df1-909a-8214cd252ed8)


---

###  3. **Why Oracle Introduced AJD**

To attract MongoDB developers and workloads to Oracle Cloud by:

* Providing **MongoDB compatibility** via Mongo API
* Offering **enterprise-grade security, performance & governance**
* Bridging **NoSQL flexibility** with **SQL power** (You can run both types of queries!)

---

###  4. **Use Cases for AJD**

* Rapidly evolving apps (e.g., IoT, mobile apps, gaming, fintech)
* JSON-heavy APIs and microservices
* Legacy MongoDB modernization
* Hybrid relational + document apps

---

###  5. **Official Resources to Read Today**

1.  [AJD Product Page](https://www.oracle.com/autonomous-database/json/)
2.  [Autonomous JSON Database Docs](https://docs.oracle.com/en/cloud/paas/autonomous-json-database/index.html)
3.  [Oracle Database API for MongoDB](https://docs.oracle.com/en/database/oracle/oracle-database/21/mongodb-user/index.html)
4.  [AJD vs ADB - Technical Comparison PDF](https://www.oracle.com/a/tech/docs/ajd-overview.pdf)

---

### Action Items (Today)

| Task                                                              | Status |
| ----------------------------------------------------------------- | ------ |
| Watch 1 short Oracle AJD intro video (YouTube)                    | ⬜      |
| Read AJD official docs intro (15–20 min)                          | ⬜      |
| Make personal notes: What’s new, what’s similar to Mongo          | ⬜      |
| Prepare 3 bullet reasons: Why someone would pick AJD over MongoDB | ⬜      |

---


