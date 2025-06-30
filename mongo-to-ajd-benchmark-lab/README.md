
---

````markdown
# MongoDB vs Oracle AJD – Real-World Benchmark Lab

This repository contains a complete end-to-end lab setup to benchmark **MongoDB running on Azure** versus **Oracle Autonomous JSON Database (AJD)** on OCI. The goal is to simulate a real-world application workload, generate consistent traffic, and gather performance insights that can guide platform migration decisions.

---

## Objectives

- Deploy a containerized full-stack application (MERN stack) backed by MongoDB on Azure.
- Generate artificial, yet realistic traffic using `k6` to simulate user activity.
- Collect detailed performance metrics from the database and application layers.
- Migrate the database to Oracle Autonomous JSON Database (AJD).
- Rerun the same test workload and compare results.
- Analyze the performance, elasticity, and cost-efficiency of both platforms.

---

## Directory Structure

```bash
mongo-to-ajd-benchmark-lab/
├── README.md                  # This file
├── docs/                      # Architecture, lab steps, and diagrams
├── terraform/                 # Infrastructure provisioning (optional)
├── app/                       # MERN stack application (frontend + backend)
├── data/                      # Sample dataset & seed scripts
├── loadtest/                  # k6 load test scripts and results
├── notebooks/                 # Jupyter notebooks for analysis
└── scripts/                   # Monitoring & automation scripts
````

---

## Technology Stack

| Layer        | Stack / Tools Used                                    |
| ------------ | ----------------------------------------------------- |
| Application  | Node.js (Express), React, Docker Compose              |
| Database     | MongoDB 6.x (Azure VM) → Oracle Autonomous JSON (OCI) |
| Load Testing | [k6 by Grafana](https://k6.io/)                       |
| Monitoring   | mongostat, mongotop, Azure Monitor, OCI Metrics       |
| Infra (Opt.) | Azure CLI, OCI CLI, Terraform                         |

---

## Benchmark Stages

### Stage 1 – MongoDB Baseline on Azure

* Provision Azure VM and deploy MongoDB.
* Run MERN sample app using Docker Compose.
* Import \~5 GB dataset (mflix).
* Generate 10-minute load using `k6`.
* Capture and export MongoDB metrics.

See [`docs/stage1-baseline.md`](docs/stage1-baseline.md)

---

### Stage 2 – Oracle AJD Migration + Load Replay

* Create AJD in Oracle Cloud.
* Migrate MongoDB data using export → DBMS\_CLOUD or Oracle's Mongo API.
* Deploy same application, point to AJD.
* Replay the exact traffic using `k6`.
* Collect AJD metrics (AWR, SQL Monitor, OCI Dashboard).

See [`docs/stage2-ajd.md`](docs/stage2-ajd.md)

---

### Stage 3 – Results Analysis

* Compare key metrics (latency, throughput, CPU, cost).
* Visualize in Jupyter Notebook.
* Summarize findings and key takeaways.

See [`docs/stage3-comparison.md`](docs/stage3-comparison.md)

---

## Sample Results (To Be Added)

* `loadtest/results-mongo/`: Mongo stats, k6 JSON
* `loadtest/results-ajd/`: AJD logs, k6 output, AWR extracts

---

## License

MIT License © 2025 Rahul Chaubey

---

## Contributing

Contributions, improvements, and issue reports are welcome. Feel free to fork or raise a PR to improve any stage of the lab.

---

## Contact

For collaboration or questions, reach out via [LinkedIn](https://www.linkedin.com/in/chaubeyrahul/) or email at: `rahulchaubey@outlook.com`

```


