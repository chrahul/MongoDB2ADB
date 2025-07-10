**Lab#2: Benchmarking Oracle Autonomous JSON Database (AJD) on OCI using YCSB – Large-Scale Setup** 

---

# **Lab#2: Benchmarking Oracle AJD on OCI using YCSB – Large-Scale Setup**

###  Objective:

To benchmark **Oracle Autonomous JSON Database (AJD)** on **Oracle Cloud Infrastructure (OCI)** using the **YCSB (Yahoo! Cloud Serving Benchmark)** tool with **large-scale data**, emulating real-world, high-throughput workloads and comparing results later against MongoDB on a similar setup.

---

##  Lab Requirements

| Component          | Configuration                                                                     |
| ------------------ | --------------------------------------------------------------------------------- |
| **VM Type**        | Oracle Linux 8.x, 2 vCPU, 8 GB RAM                                                |
| **VM Shape**       | `VM.Standard3.Flex` (min), can scale up for larger datasets                       |
| **Block Storage**  | 100 GB OCI Block Volume                                                           |
| **Database**       | Autonomous JSON Database (AJD) provisioned on OCI                                 |
| **YCSB Tool**      | Version `0.18.0` compiled with JDBC binding                                       |
| **Wallet**         | Oracle Wallet downloaded from AJD Console (contains `tnsnames.ora`, `.jks`, etc.) |
| **JARs Required**  | `ojdbc11.jar`, `oraclepki.jar`, `osdt_core.jar`, `osdt_cert.jar`                  |
| **Data Volume**    | **Large scale**: `recordcount=1000000`, `operationcount=1000000`                  |
| **Benchmark Type** | **Workload A** – Balanced read/write workload                                     |

---

##  Step-by-Step Setup

### 1️ Provision AJD on OCI

* Go to OCI Console → **Autonomous JSON DB** → Create new DB

  * Choose workload type: **JSON**
  * Size: Keep default
  * **Auto scaling**: Enabled
  * Download the **Wallet** after DB is provisioned
  * Note the **Service Name** (e.g., `ajd_medium`) from `tnsnames.ora`

---

### 2️ Create Oracle Linux VM

* **Shape**: `VM.Standard3.Flex` (2 OCPU, 8GB RAM)
* Attach **Block Volume**: 100 GB
* Update OS:

  ```bash
  sudo yum update -y
  ```

---

### 3️ Install Java and Maven

```bash
sudo yum install java-11-openjdk-devel git -y
sudo yum install maven -y
```

---

### 4️ Install YCSB

```bash
cd ~
git clone https://github.com/brianfrankcooper/YCSB.git
cd YCSB
mvn -pl site.ycsb:jdbc-binding -am clean package -DskipTests
```

---

### 5️ Copy Required JARs to lib folder

Place the following into the YCSB JDBC binding lib directory:

```bash
cp /path/to/ojdbc11.jar jdbc/target/ycsb-jdbc-binding-*/lib/
cp /path/to/oraclepki.jar jdbc/target/ycsb-jdbc-binding-*/lib/
cp /path/to/osdt_core.jar jdbc/target/ycsb-jdbc-binding-*/lib/
cp /path/to/osdt_cert.jar jdbc/target/ycsb-jdbc-binding-*/lib/
```

---

### 6️ Unzip and Configure Wallet

```bash
unzip Wallet_<DB_NAME>.zip -d ~/YCSB/wallet
chmod 600 ~/YCSB/wallet/*
```

* Validate the contents:

```bash
ls ~/YCSB/wallet
```

---

### 7️ Configure `oracle-ajd.properties`

Create the properties file for YCSB connection:

```properties
# ~/YCSB/jdbc/oracle-ajd.properties

db.driver=oracle.jdbc.OracleDriver
db.url=jdbc:oracle:thin:@ajd_medium?TNS_ADMIN=/root/YCSB/wallet
db.user=ADMIN
db.passwd=YourPassword123
jdbc.batchupdateapi=true
recordcount=1000000
operationcount=1000000
```

---

### 8️ Export Required JAVA\_OPTS

```bash
export JAVA_OPTS="-Doracle.net.tns_admin=/root/YCSB/wallet"
```

---

### 9️ Run Load Phase (Insert Data)

```bash
cd ~/YCSB
./jdbc/target/ycsb-jdbc-binding-*/bin/ycsb load jdbc -s \
  -P jdbc/oracle-ajd.properties \
  -P workloads/workloada
```

* **Monitor**: This may take several minutes for 1 million records.

---

###  Run Transaction Phase (Read/Update Ops)

```bash
./jdbc/target/ycsb-jdbc-binding-*/bin/ycsb run jdbc -s \
  -P jdbc/oracle-ajd.properties \
  -P workloads/workloada
```

---

##  Output Sample (Interpreting Results)

You will receive key metrics like:

```text
[OVERALL], RunTime(ms), 142943
[OVERALL], Throughput(ops/sec), 699.9
[READ], AverageLatency(us), 8750
[UPDATE], AverageLatency(us), 9271
[INSERT], Return=OK, 1000000
```

* **Throughput**: Higher = better
* **Latency**: Lower = better
* **Percentile Latency**: Helps assess tail performance

---

##  Pro Tips & Troubleshooting

| Issue                                              | Resolution                                             |
| -------------------------------------------------- | ------------------------------------------------------ |
| `ORA-17957: Unable to initialize keystore`         | Ensure wallet files are readable and paths are correct |
| `NoClassDefFoundError` or `ClassNotFoundException` | Ensure all 4 Oracle JARs are present in `lib/`         |
| Performance seems slow                             | Use a larger VM shape like 4 vCPU / 16 GB RAM          |

---

##  Optional – Capture JSON Load in DB

You can use SQL Developer Web to:

```sql
SELECT COUNT(*) FROM YCSB_USERTABLE;
SELECT * FROM YCSB_USERTABLE WHERE YCSB_KEY = 'user292199';
```

---

##  Optional – Repeat With Other Workloads

Change workload file:

```bash
-P workloads/workloadb  # Read-heavy
-P workloads/workloadd  # Read-latest
```

---

##  Outcome

This lab generates empirical results on AJD’s performance under high-volume JSON operations using YCSB. The results can now be compared to **MongoDB’s performance on Azure VM** from Lab#1 for fair evaluation.

---
