
---

# **Day 3: Why Customers Are Migrating Away from MongoDB**

---

## Objective:

To understand the **real-world limitations of MongoDB** that push enterprises to look for alternatives like Oracle Autonomous JSON Database (AJD).

---

## Core Issues Driving Migration

Oracle groups the challenges into three broad categories:

---

### 1. **Financial Issues**

| Challenge               | 💬 Description                                                                              |
| -------------------------- | ------------------------------------------------------------------------------------------- |
| **High Operational Costs** | Managing MongoDB clusters (replica sets, sharding) requires skilled DBAs and infrastructure |
| **High Licensing Costs**   | Enterprise editions (e.g., MongoDB Atlas) come with significant subscription fees           |

> *Example:* Running MongoDB Atlas with high availability and backup features can get expensive quickly, especially for large datasets and multiple regions.

---

###  2. **People/Skills Issues**

| Challenge                  |  Description                                                                                                |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **Multiple Skillsets Needed** | Teams need to manage **different databases** (MongoDB, SQL DBs, analytics DBs) — more training, more ops load |
| **Training Overhead**         | MongoDB's distinct query language and tuning techniques require **dedicated training**                        |

> *Consequence:* Organizations spend more time hiring, training, and retaining engineers with multiple database specializations.

---

###  3. **Technical Limitations of MongoDB**

| Limitation                      | 📉 Impact                                                                                                 |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------- |
|  **Single-Purpose Data Model**    | Harder to join/relate data → leads to data duplication and fragmented architecture                        |
|  **No Native SQL Support**        | Cannot run cross-collection joins or relational queries easily                                            |
|  **Limited Analytics**            | No built-in analytics or reporting without exporting data                                                 |
|  **Complex Architecture**         | Performance tuning is tough due to **sharding**, **replica sets**, and **high memory** use                |
|  **Security Gaps**                | Enterprise-class security is limited in comparison to Oracle (e.g., encryption, auditing)                 |
|  **Read Consistency & Atomicity** | MongoDB is **eventually consistent**; does not support ACID transactions for multi-doc updates by default |

---

## Real-World Impact:

MongoDB applications often evolve from small dev projects to complex production workloads. As they grow:

* Costs go up (infra + licensing)
* Security & analytics needs grow
* Data modeling gets painful
* Lack of joins hurts data quality
* IT overhead increases

>  **Result:** Enterprises realize they need an **all-in-one, enterprise-grade database** that can handle **JSON + SQL + analytics + security** — hence the shift to Oracle AJD.

---

## Summary Table

| MongoDB Weakness                 | What Oracle AJD Offers     |
| ---------------------------------- | ---------------------------- |
| No native SQL joins                | Full SQL + JSON access       |
| Performance bottlenecks            | Auto-tuning + indexing       |
| Manual tuning required             | Autonomous features          |
| Replica-based design               | High availability (RAC, ADG) |
| Eventual consistency               | Full ACID transactions       |
| No built-in analytics              | In-database ML + analytics   |
| Extra cost for enterprise features | Built-in in Autonomous DB    |

---

## Your Tasks for Day 3:

| Task        | Description                                                                                   | Done? |
| ----------- | --------------------------------------------------------------------------------------------- | ----- |
| Read     | Page 4 of the Oracle guide thoroughly                                                         | ⬜     |
| Write    | `docs/day3_why_migrate_from_mongo.md`                                                         | ⬜     |
| Capture  | At least 5 clear reasons why MongoDB is no longer sufficient for growing enterprise workloads | ⬜     |
| Add       | Comparative Table in your notes (MongoDB vs Oracle AJD)                                       | ⬜     |
| Optional | Want a diagram showing **evolving architecture** from MongoDB to Converged DB?                | ⬜     |

---
