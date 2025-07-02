
---

###  **Objective**

Benchmark insert/read/update performance of Oracle AJD using the [Yahoo! Cloud Serving Benchmark (YCSB)](https://github.com/brianfrankcooper/YCSB).

---

###  **Pre-requisites**

| Requirement                                           | Purpose                            |
| ----------------------------------------------------- | ---------------------------------- |
|  Oracle Cloud Account                                | To provision AJD                   |
|  AJD instance                                        | Must be provisioned and accessible |
|  VCN + Public IP/Access                              | So that your container can connect |
|  Linux machine with Docker                           | To run the YCSB container          |
|  Git & Maven installed (for local build alternative) | Optional if not using Docker       |

---

###  **Benchmarking Approach**

We will:

* Use [YCSB](https://github.com/brianfrankcooper/YCSB) and its JDBC binding
* Build a custom Docker image (YCSB + Maven + MongoDB or JDBC bindings)
* Run the benchmark workloads via container
* Point the workload to your AJD using Oracle JDBC URL

---

###  **Steps to Benchmark AJD**

---

#### 1️ Clone YCSB GitHub repo

```bash
git clone https://github.com/brianfrankcooper/YCSB.git
cd YCSB
```

GitHub link: [https://github.com/brianfrankcooper/YCSB](https://github.com/brianfrankcooper/YCSB)

---

#### 2️ \[Optional] Modify `pom.xml` to add Oracle JDBC Driver support

Oracle JDBC driver is **not available via Maven Central**, so you may need to manually install it into your local Maven repo or use a volume mount if needed.
(Or switch to `MongoDB` binding for simpler testing.)

---

#### 3️ Create a Dockerfile (use MongoDB or JDBC binding)

##### Example: For MongoDB binding

```Dockerfile
FROM maven:3.9.7-eclipse-temurin-11 AS build
WORKDIR /ycsb
COPY . .
RUN mvn -pl mongodb -am -B -q clean package

FROM eclipse-temurin:11-jre-jammy
WORKDIR /ycsb
COPY --from=build /ycsb/bin ./bin
COPY --from=build /ycsb/workloads ./workloads
COPY --from=build /ycsb/mongodb/target ./mongodb/target
ENTRYPOINT ["/bin/bash"]
```

---

#### 4️ Build Docker Image

```bash
docker build -t ycsb:mongodb .
```

---

#### 5️ Run YCSB Container to Benchmark

Use `load` for inserting data and `run` for benchmarking read/update.

Example: MongoDB binding (adjust for JDBC/AJD accordingly)

```bash
docker run --rm -it ycsb:mongodb \
  ./bin/ycsb load mongodb -s \
  -P workloads/workloada \
  -p mongodb.url="mongodb://<host>:<port>/<db>"
```

---

#### 6️ \[For Oracle AJD]

If using Oracle Autonomous JSON DB, follow this:

* Download Oracle Instant Client + JDBC thin driver
* Use the **JDBC binding** in YCSB
* Provide connection string like:

```bash
jdbc:oracle:thin:@<AJD_TNS_NAME>?TNS_ADMIN=/path/to/your/wallet
```

Example command (adjust paths accordingly):

```bash
./bin/ycsb load jdbc -s -P workloads/workloada \
  -p db.driver=oracle.jdbc.OracleDriver \
  -p db.url="jdbc:oracle:thin:@ajd_high?TNS_ADMIN=/ycsb/wallet" \
  -p db.user=admin \
  -p db.passwd=YourPassword
```

---

###  **Output Sample**

```
[READ], Operations, Return=OK, 4999
[UPDATE], Operations, Return=OK, 4870
[INSERT], Operations, Return=OK, 5000
[CLEANUP], Operations, Return=OK, 1
Run finished, takes 8.3522131 seconds
```

---

###  \[GitHub Reference Links]

*  YCSB Main Repo: [https://github.com/brianfrankcooper/YCSB](https://github.com/brianfrankcooper/YCSB)
*  MongoDB binding: [`mongodb/`](https://github.com/brianfrankcooper/YCSB/tree/master/mongodb)
*  JDBC binding: [`jdbc/`](https://github.com/brianfrankcooper/YCSB/tree/master/jdbc)
*  Sample Workloads: [`workloads/`](https://github.com/brianfrankcooper/YCSB/tree/master/workloads)

---

###  OCI Wallet Config for JDBC (AJD)

* Download **wallet.zip** from OCI AJD Console
* Unzip it and set `TNS_ADMIN=/path/to/unzipped/wallet`

---


