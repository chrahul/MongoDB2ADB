
---

# **Lab #3: Large-Scale Benchmarking of Oracle Autonomous JSON Database (AJD) using YCSB**

---

##  **Objective**

Benchmark Oracle AJD's performance using **Yahoo! Cloud Serving Benchmark (YCSB)** with **1 million records and 1 million operations**, simulating a realistic, high-load application environment. The results will help us compare AJD with other NoSQL document databases like MongoDB.

---

##  **Lab Requirements**

| Component           | Configuration                                    |
| ------------------- | ------------------------------------------------ |
| **VM (on OCI)**     | 2 vCPU, 8 GB RAM (e.g., `VM.Standard3.Flex`)     |
| **OS**              | Oracle Linux 8.x                                 |
| **Storage**         | 100 GB Block Volume                              |
| **DB Target**       | Oracle Autonomous JSON Database (Medium service) |
| **Wallet**          | Downloaded and unzipped in: `/root/YCSB/wallet`  |
| **YCSB Version**    | `0.18.0`                                         |
| **JDK**             | Java 11 or 17                                    |
| **Threads**         | 20                                               |
| **Record Count**    | 1,000,000                                        |
| **Operation Count** | 1,000,000                                        |

---

##  **Pre-Requisites Setup**

### 1️ Install Required Packages

```bash
sudo yum install git java-11-openjdk maven unzip -y
```

### 2️ Clone and Build YCSB

```bash
cd ~
git clone https://github.com/brianfrankcooper/YCSB.git
cd YCSB
mvn -pl jdbc-binding -am clean package
```

---

##  **Directory Structure Reference**

```
~/YCSB/
├── jdbc/
│   └── target/ycsb-jdbc-binding-0.18.0-SNAPSHOT/
│       ├── lib/
│       │   ├── ojdbc11.jar
│       │   ├── oraclepki.jar
│       │   ├── osdt_cert.jar
│       │   └── osdt_core.jar
├── workloads/
│   └── workload_large
├── jdbc/oracle-ajd.properties
└── wallet/  (Oracle Wallet unzipped here)
```

---

##  **Step-by-Step Wallet Configuration**

### 3️ Unzip Oracle Wallet

Download from OCI Console and unzip into:

```bash
mkdir -p ~/YCSB/wallet
unzip Wallet_WSMNGDB.zip -d ~/YCSB/wallet
```

### 4️ Set Permissions

```bash
chmod 600 ~/YCSB/wallet/*
```

### 5️ ojdbc.properties (No need to modify unless using JKS)

Check it includes:

```properties
oracle.net.wallet_location=(SOURCE=(METHOD=FILE)(METHOD_DATA=(DIRECTORY=/root/YCSB/wallet)))
oracle.net.tns_admin=/root/YCSB/wallet
```

---

##  **Step-by-Step: Create Config Files**

### 6️ `oracle-ajd.properties` (JDBC Settings)

Create:

```bash
nano ~/YCSB/jdbc/oracle-ajd.properties
```

Paste:

```properties
db.driver=oracle.jdbc.OracleDriver
db.url=jdbc:oracle:thin:@wsmngdb_medium?TNS_ADMIN=/root/YCSB/wallet
db.user=admin
db.pass=your_ajd_password
db.batchsize=100
jdbc.fetchsize=10
jdbc.autocommit=true
jdbc.usebatchupdateapi=true
```

---

### 7️ `workload_large` (Benchmark Settings)

Create:

```bash
nano ~/YCSB/workloads/workload_large
```

Paste:

```properties
recordcount=1000000
operationcount=1000000
workload=com.yahoo.ycsb.workloads.CoreWorkload
readproportion=0.5
updateproportion=0.5
insertproportion=0
scanproportion=0
requestdistribution=zipfian
```

---

##  **Step-by-Step: Benchmark Execution**

### 8️ Load Phase (Inserts 1 Million Records)

```bash
cd ~/YCSB
./jdbc/target/ycsb-jdbc-binding-*/bin/ycsb load jdbc -s \
  -P jdbc/oracle-ajd.properties \
  -P workloads/workload_large \
  -threads 20 \
  -p status.interval=5 \
  > ycsb_ajd_load.log 2>&1
```

###  Validate:

```bash
grep "\[INSERT\]" ycsb_ajd_load.log | tail
```

---

### 9️ Run Phase (Performs Read/Update Operations)

```bash
./jdbc/target/ycsb-jdbc-binding-*/bin/ycsb run jdbc -s \
  -P jdbc/oracle-ajd.properties \
  -P workloads/workload_large \
  -threads 20 \
  -p status.interval=5 \
  > ycsb_ajd_run.log 2>&1
```

###  Validate:

```bash
grep "\[READ\]" ycsb_ajd_run.log | tail
grep "\[UPDATE\]" ycsb_ajd_run.log | tail
```

---

##  **Important Metrics Captured**

* **Throughput**: `ops/sec`
* **Latency**: Min / Max / Average / 50th / 95th / 99th percentile
* **Runtime**: Total duration of test
* **GC Stats**: Young/Old Generation GC count and time

---

##  **Troubleshooting Tips**

| Issue              | Fix                                                                |
| ------------------ | ------------------------------------------------------------------ |
| Test ends in < 60s | Check if `recordcount` and `operationcount` were actually applied. |
| Zero inserts       | Ensure fresh table. Drop table before rerun.                       |
| SSL errors         | Recheck wallet path & permissions.                                 |
| High max latency   | AJD first-time TCPS overhead is expected. Ignore one-off spikes.   |

---

##  **Optional: Drop Table Before Each Test**

Run from SQL Developer or SQL\*Plus:

```sql
DROP TABLE usertable;
```

---

##  **Outputs to Archive**

1. `ycsb_ajd_load.log`
2. `ycsb_ajd_run.log`
3. Insert/Read/Update summary tables

---

