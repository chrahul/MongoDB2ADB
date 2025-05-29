
---

# Day 9: Stage 4 – Validation Phase Deep Dive

---

## Goal

To **verify, test, and confirm** that the migration to Oracle AJD was successful across all dimensions:

* Data accuracy
* Application functionality
* Performance
* Stakeholder satisfaction

---

## Step by Step: Validation Phase

---

### 1. Data Verification

Confirm that the data in Oracle matches MongoDB exactly

| Task            | Description                                                 |
| --------------- | ----------------------------------------------------------- |
| Compare JSON    | Document-level comparison using checksums or custom scripts |
| Validate Counts | Match collection or row counts                              |
| Validate Keys   | Ensure primary fields like IDs, timestamps are preserved    |
| Validate Types  | Check that JSON values did not lose precision or structure  |

Tools:

* Python or JavaScript scripts to read from both systems
* SQL queries against Oracle collections
* Sample document comparisons

---

### 2. Functional Testing

Ensure your application works correctly with Oracle AJD

| What to Test          | Examples                                 |
| --------------------- | ---------------------------------------- |
| API Calls             | All create read update delete operations |
| Aggregation Pipelines | `$match`, `$group`, `$sort`              |
| Application Screens   | End user functionality                   |

Focus on:

* CRUD behavior
* Filters and queries
* Form submissions or background jobs

---

### 3. Performance Testing

Measure how the system behaves under real-world or simulated loads

| What to Monitor   | How                                              |
| ----------------- | ------------------------------------------------ |
| Query latency     | Use Oracle AWR or monitoring dashboards          |
| App response time | Simulate concurrent users with JMeter or Postman |
| CPU and memory    | Use OCI metrics or SQL Developer Web             |
| Index efficiency  | Review query execution plans                     |

---

### 4. User Acceptance Testing

Involve end users to confirm that the system works as expected

| Step            | Action                               |
| --------------- | ------------------------------------ |
| Prepare scripts | List user workflows to test          |
| Gather feedback | Note usability gaps or slowness      |
| Approve         | Get business or stakeholder sign-off |

---

### 5. Post Migration Monitoring

Set up a framework to observe the system for the first few weeks after cutover

| What to Watch      | Tools                                |
| ------------------ | ------------------------------------ |
| Error logs         | SQL Developer Web or cloud logging   |
| Slow queries       | Oracle Automatic Workload Repository |
| Uptime and latency | OCI Monitoring                       |
| User complaints    | Create a temporary feedback form     |

---

## Real World Tip

Keep a short freeze period where no schema or logic changes are allowed immediately after migration. This gives time to monitor and address hidden issues.

---

## Validation Phase Summary

| Area        | What to Check                            |
| ----------- | ---------------------------------------- |
| Data        | Accuracy, type match, document count     |
| App         | Functional testing, aggregation behavior |
| Performance | Load handling, latency, scalability      |
| UAT         | Final sign-off from business teams       |
| Monitoring  | Post migration alerts and logs           |

---

## Your Tasks for Day 9

| Task   | Description                                |
| ------ | ------------------------------------------ |
| Read   | Page 12 of the guide (Validation section)  |
| Create | `docs/day9_validation_phase.md`            |
| List   | Your validation checklist (data, app, UAT) |
| Tools  | Mention any tools or scripts used          |
| UAT    | Outline sample user flow you want to test  |

---


