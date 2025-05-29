
---

# **Day 5: The PMG 4-Phase Migration Journey**

---

## Objective:

Understand Oracle’s structured migration approach — the **PMG Process** — consisting of **four phases**:

> **Planning → Preparation → Execution → Validation**

This method ensures **minimum risk, high success**, and a **repeatable process** for any MongoDB to Oracle migration.

---

## Overview of the 4 Phases

---

### **1. Planning Phase**

The foundation for the migration journey.

#### Key Goals:

* Define **business and technical goals**
* Identify **stakeholders**
* Perform **source system assessment**

#### Activities:

| Task                             | Description                                                     |
| -------------------------------- | --------------------------------------------------------------- |
| Goal Definition                | Performance, cost, scale, or capability improvement?            |
| Stakeholder Identification     | Involve DBAs, devs, architects, business users                  |
| Current Environment Assessment | Review MongoDB workload size, indexes, complexity, dependencies |

> **Discovery Questions** are provided in Appendix 1 — ask about current infra, goals, data complexity, DR, PII, etc.

---

### **2. Preparation Phase**

Prepare the **target system** and validate **migration readiness**.

#### Key Activities:

| Task                          | Details                                                          |
| ----------------------------- | ---------------------------------------------------------------- |
| Setup Target System       | Provision Oracle Autonomous JSON DB or 23ai                      |
| Define Migration Strategy  | Choose method (GoldenGate, External Tables, Flat Files, etc.)    |
| Perform Prerequisite Checks | Version compatibility, required drivers (MongoSH, PyMongo, etc.) |
| Create Test Cases          | Functional, integration, and UAT test scenarios                  |
| Rollback Plan              | Define and document rollback/fallback plan                       |
| Backup Source System       | Use `mongodump` (with `--oplog`) to snapshot MongoDB data        |
| Setup Infra Components     | VMs, Object Storage, IAM, networking, logging, automation tools  |

---

### **3. Execution Phase**

Migrate the **data**, **database objects**, and **application logic**.

#### Key Activities:

| Task                  | Description                                                          |
| --------------------- | -------------------------------------------------------------------- |
| Object Migration   | Migrate schemas, indexes, collections                                |
| Data Migration     | Use preferred tools (Oracle Loader, JSON External Tables, OGG, etc.) |
| Application Update | Modify app code to work with Oracle MongoDB API (minimal changes)    |

> Use the **MongoDB Compatibility Advisor** to analyze logs for unsupported operators or aggregation steps. (Python tool provided in guide)

---

###  **4. Validation Phase**

Verify correctness, performance, and user satisfaction.

#### Activities:

| Task                             | Description                                       |
| -------------------------------- | ------------------------------------------------- |
| Data Validation                | Compare source vs target data                     |
| Functional Testing            | Ensure apps work without regressions              |
| Performance Testing           | Check query speed, transaction response           |
| User Acceptance Testing (UAT) | Stakeholder-level validations                     |
| Post-Migration Monitoring     | Watch for errors, latency, slow queries over time |

---

## PMG Migration Flow Summary

| Phase          | Key Outcome                                 |
| -------------- | ------------------------------------------- |
| Planning    | Vision, stakeholders, what to migrate, why  |
| Preparation | Target system setup, strategy, tools, tests |
| Execution   | Actual data + code migration                |
| Validation  | Testing, performance, user acceptance       |

---

##  Your Tasks for Day 5:

| Task         | Description                                                   | Done? |
| ------------ | ------------------------------------------------------------- | ----- |
| Read      | Pages 6–7 of the Oracle guide (PMG Process)                   | ⬜     |
| Write     | `docs/day5_migration_journey.md`                              | ⬜     |
| Summarize | The 4 phases with: objectives + key tasks + your custom notes | ⬜     |
| Optional  | Want a **visual flowchart of PMG Migration Journey**?         | ⬜     |

---

Next up for **Day 6**:
We will go **deep into Stage 1 – Planning**, including Discovery Questions and how to assess MongoDB workloads.
