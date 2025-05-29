# MongoDB to Oracle Autonomous JSON Database (AJD) Migration Blueprint

---

## 1. Executive Summary

This document outlines the complete end-to-end migration journey of MongoDB workloads to Oracle Autonomous JSON Database (AJD). It follows Oracle’s Platform Migration Group (PMG) 4-phase approach and includes discovery findings, tool selection, execution steps, testing, and final outcomes.

**Key Goals:**
- Simplify infrastructure by consolidating MongoDB into Oracle
- Enable both document and SQL access to JSON data
- Improve security, manageability, and scalability using AJD
- Minimize downtime and manual effort via automation

---

## 2. Source Workload Overview

| Property | Value |
|----------|-------|
| MongoDB Version | 4.4 |
| Deployment | On-prem |
| Number of Collections | 15 |
| Total Data Size | 12 GB |
| Indexes Used | 21 |
| Application Stack | Node.js with Mongoose ODM |
| Aggregation Features | `$match`, `$group`, `$set`, `$lookup` (limited) |

---

## 3. Target Environment: Oracle AJD

| Property | Value |
|----------|-------|
| Target Database | Oracle Autonomous JSON Database |
| Oracle Version | 23ai |
| Cloud Region | OCI India West (Mumbai) |
| Scaling Mode | Auto-scaling |
| Access Methods | Mongo Shell, SQL Developer Web |
| Security | Oracle Vault, TLS, IAM Role-based Access |

---

## 4. Migration Strategy

We adopted Oracle’s **PMG 4-Phase Migration Framework**:

| Phase | Summary |
|-------|---------|
| Planning | Goal setting, stakeholder alignment, workload audit |
| Preparation | AJD provisioning, backups, compatibility assessment |
| Execution | Data export and load using JSON + external tables |
| Validation | Data and functional verification, UAT, performance tests |

---

## 5. Tools and Methods Used

| Tool | Purpose |
|------|---------|
| `mongodump` | Backup of live MongoDB |
| `mongoexport` | Export JSON documents |
| Oracle External Tables | Load JSON into AJD |
| DBMS_CLOUD.copy_data | Load from Object Storage |
| Compatibility Advisor | Query and operator validation |
| SQL Developer Web | Monitoring and schema creation |

---

## 6. Compatibility Analysis

**Tool Used:** MongoDB Compatibility Advisor  
**Result:** 97.5 percent compatibility

| Supported Features | Unsupported Features |
|--------------------|----------------------|
| Basic CRUD | `$mapReduce` |
| Aggregation Pipelines | Deep graph-based joins |
| Index creation | N/A |
| JSON data types | Few BSON edge types (like `MinKey`) |

---

## 7. Validation Summary

| Test | Result |
|------|--------|
| Document Count Check | Passed |
| Index Efficiency | Re-created indexes manually |
| Query Latency | Improved by 25 percent post-migration |
| UAT | All user workflows tested and validated |
| Data Integrity | Checksums and counts matched 1 to 1 |

---

## 8. Post-Migration Observations

- Oracle AJD scaled seamlessly under load
- Monitoring setup using OCI built-in alerts
- Wallet-based secure connectivity tested
- No data corruption or missed collections
- Cost and compute usage stabilized after 3 days

---

## 9. Risks and Lessons Learned

| Risk/Challenge | Resolution |
|----------------|------------|
| `$lookup` compatibility | Rewritten using app-side joins |
| Index mismatch | Used SQL to recreate composite indexes |
| JSON array depth issues | Flattened schema before import |
| App compatibility | Retained MongoDB driver; no rewrite needed |

---

## 10. Appendices

- `mongodump` and `mongoexport` command examples
- JSON table creation SQL script
- Compatibility Advisor sample output
- AJD Wallet setup steps
- Visual diagrams (PMG Process, AJD Architecture)

---

## Author & Lab Owner

- Created and validated by: *Rahul Chaubey*
- Date: *DD-MM-YYYY*
- Repo: [`mongodb-ajd-lab-series`](../mongodb-ajd-lab-series)

