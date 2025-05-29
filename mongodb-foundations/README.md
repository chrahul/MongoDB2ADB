
---

# MongoDB Foundations

This folder contains foundational learning content and setup steps for the **MongoDB to Oracle Autonomous JSON Database (AJD) Migration Lab**.

It is designed to help you build a solid understanding of MongoDB concepts before transitioning to hands-on migration activities. Each file in this folder walks through key MongoDB topics, setup runbooks, and Oracle integration preparation steps.

---

![image](https://github.com/user-attachments/assets/67fe999c-e296-4926-a3d2-43699a901f2a)


## What’s Included

| File                                          | Description                                            |
| --------------------------------------------- | ------------------------------------------------------ |
| `01_What is MongoDB.md`                       | Introduction to MongoDB and its document model         |
| `02_What isMongoDB.md`                        | Conceptual understanding of MongoDB database engine    |
| `03_Understanding Documents.md`               | Deep dive into BSON and document structure             |
| `04_MongoDB Product Suite.md`                 | Overview of MongoDB tools and components               |
| `05_Distributed Architecture of MongoDB.md`   | Replica sets and sharding basics                       |
| `06_00_Summary.md`                            | Quick summary before entering runbooks                 |
| `06_01_Runbook_MongoDB Setup.md`              | Step-by-step setup of MongoDB locally or in the cloud  |
| `07_MongoDB Structure Hierarchy.md`           | Explains database, collections, and document hierarchy |
| `08_High-Level Plan for MongoDB Migration.md` | Strategy outline for migrating to Oracle AJD           |
| `09_mongoexport_mongoimport.md`               | CLI-based export and import commands                   |
| `10_Runbook_AJD Preparation.md`               | How to prepare Oracle Autonomous JSON DB               |
| `11_Read_The_Oracle_Blog.md`                  | Learning summary from official Oracle blogs            |
| `12_Create Autonomous Database.md`            | Instructions to provision AJD on OCI                   |
| `13_Download Wallet and Prepare.md`           | Steps to enable secure Oracle DB connection            |

---

## How to Use

1. Start from file `01` and work your way down.
2. Follow each runbook carefully and try to execute commands in your lab.
3. Use files `08` to `13` as transition points into Oracle setup and migration readiness.
4. This module prepares you for executing the full MongoDB to AJD migration using the Oracle Database API for MongoDB.

---

## Prerequisites

* Basic familiarity with JSON and NoSQL concepts
* MongoDB installed or accessible
* Oracle Cloud account (for AJD setup)
* SQL Developer or SQL Developer Web access

---

## Next Steps

After completing this section, move to the next directory in your repository that contains:

* Compatibility checks
* Actual data migration execution
* Validation and post-migration tuning

---

