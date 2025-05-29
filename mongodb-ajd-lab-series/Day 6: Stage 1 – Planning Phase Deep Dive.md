
---

# **Day 6: Stage 1 – Planning Phase Deep Dive**

---

## Objective:

To master the **Planning Phase** of the migration process by:

1. Conducting effective discovery sessions
2. Mapping existing MongoDB workloads
3. Defining goals, constraints, and strategy

---

## Step-by-Step: Stage 1 — Planning

---

### 1. Define Goals

Ask: *Why are we migrating this MongoDB workload to Oracle AJD?*

| Common Goals                   |
| --------------------------------- |
| Cost reduction                    |
| Platform consolidation            |
| Need for SQL/analytics on JSON    |
| Better security & compliance      |
| Autonomous infrastructure         |
| Scalability and high availability |

---

### 2. Identify Stakeholders

List roles who will participate:

| Role                  | Involvement                    |
| ------------------------ | --------------------------------- |
| Application Dev          | Validate app behavior             |
| DBA                      | Source/target DB expertise        |
| Cloud Architect          | OCI provisioning & infra          |
| Business Owner           | Approves goals/ROI                |
| Security/Compliance Lead | Validates PII controls, IAM setup |
| UAT Team                 | Final validation                  |

---

### 3. Conduct Discovery Workshop

Use this checklist during your stakeholder sessions:

#### Sample Discovery Questions (From Appendix 1)

---

#### Business

* What are the **business drivers** for the migration?
* Do you have **executive sponsorship**?
* What is the **timeline**?
* Who will **own implementation**?
* What are your **cloud adoption levels**?

#### Technical

* What is your **existing MongoDB architecture**?
* How large is your **data volume**?
* Are there **complex data models** (nested JSON, polymorphic data)?
* What are your **HA/DR requirements**?
* Any preferred **Oracle technologies**?

#### Database

* How many **collections** (schemas) are there?
* Are there **indexes** that must be preserved?
* Any **license renewals** to consider?

#### Application

* What is the **application stack** (Node.js, Python, Java)?
* Any use of **MongoDB-specific features** like `$lookup`, `$merge`, `$graphLookup`?
* Is the workload **multi-tenant**?
* Any **ETL scripts, reports, pipelines** that use Mongo?

#### Testing

* What are your **acceptance criteria**?
* What is your **UAT plan**?
* Do you have **test datasets** available?

#### Deployment

* Do you plan a **pilot rollout** or full cutover?
* Do you need a **fallback** plan?

---

## Suggested Output: `docs/day6_planning_stage.md`

Break it into:

* Goals
* Stakeholders
* Workload summary
* Discovery summary
* Risks/constraints
* Suggested timelines

---

## Your Day 6 Tasks

| Task       | Description                                                    | Done? |
| ---------- | -------------------------------------------------------------- | ----- |
| Read    | Page 8 (Stage 1 Planning)                                      | ⬜     |
| Write   | `docs/day6_planning_stage.md`                                  | ⬜     |
| Fill    | At least a partial version of `migration-plan-template.md`     | ⬜     |
| Prepare | List of 3–5 workloads you’d want to assess using this template | ⬜     |

---

