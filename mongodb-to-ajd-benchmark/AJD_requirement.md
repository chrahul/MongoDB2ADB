 **setup requirements document**  lab environment for benchmarking **Oracle Autonomous JSON Database (AJD)** using **YCSB** from a Linux VM:

---

#  YCSB Benchmark Setup: Requirements Checklist

##  Objective

To benchmark an **Oracle Autonomous JSON Database (AJD)** using the **Yahoo! Cloud Serving Benchmark (YCSB)** from a secure **Oracle Linux VM** over **TCPS/SSL connection**.

---

##  Components Overview

###  (A) Autonomous JSON Database (AJD) on Oracle Cloud

* **DB Name:** `wsmngdb_medium` (or as named by user)
* **Type:** Autonomous JSON Database
* **Wallet:** Download *Instance Wallet* ZIP (e.g. `Wallet_WSMNGDB.zip`) after DB creation

###  (B) OCI Linux VM (Benchmark Host)

* **OS:** Oracle Linux 8 or 9 (CentOS 7/8 also OK)
* **Compute:** 2 vCPU, 8 GB RAM (minimum)
* **Public access:** Required for SSH and wallet upload

###  (C) YCSB Folder Structure on VM

```
~/YCSB/
├── jdbc/
│   └── target/
│       └── ycsb-jdbc-binding-0.18.0-SNAPSHOT/
│           ├── lib/
│           │   ├── ojdbc11.jar         ← [Required]
│           │   ├── oraclepki.jar       ← [Required]
│           │   ├── osdt_core.jar       ← [Required]
│           │   └── osdt_cert.jar       ← [Required]
│           └── bin/
├── workloads/   ← Default workload A, B, D
├── jdbc/oracle-ajd.properties   ← DB config
└── /u01/ajd_wallet/             ← Wallet unzip destination
```

---

##  Software Required on VM

1. **Java 11 JDK**

   ```bash
   sudo yum -y install java-11-openjdk-devel
   ```

2. **Maven 3.6+**

   * Install manually if yum version is old:

     ```bash
     curl -LO https://archive.apache.org/dist/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz
     tar -xz -C /opt -f apache-maven-3.9.6-bin.tar.gz
     sudo ln -sfn /opt/apache-maven-3.9.6 /opt/maven
     echo 'export PATH=/opt/maven/bin:$PATH' | sudo tee /etc/profile.d/maven.sh
     source /etc/profile.d/maven.sh
     ```

3. **Git, unzip, curl**

   ```bash
   sudo yum install git unzip curl -y
   ```

---

##  Files to Upload to VM

| File                           | Description                       |
| ------------------------------ | --------------------------------- |
| `Wallet_WSMNGDB.zip`           | Wallet for the AJD                |
| `ojdbc11.jar`                  | Oracle JDBC driver                |
| `oraclepki.jar`                | Required for wallet-based SSL     |
| `osdt_core.jar`                | Required for wallet-based SSL     |
| `osdt_cert.jar`                | Required for wallet-based SSL     |
| (Optional) `ojdbc-full.tar.gz` | Contains all driver jars for ease |

> You can download these JARs from Oracle's JDBC download page.

---

##  Post-Setup Checklist

* [ ] Unzip wallet to `/u01/ajd_wallet`
* [ ] Ensure `ojdbc.properties` inside wallet has:

  ```
  oracle.net.wallet_location=(SOURCE=(METHOD=FILE)(METHOD_DATA=(DIRECTORY=/u01/ajd_wallet)))
  oracle.net.tns_admin=/u01/ajd_wallet
  ```
* [ ] Set file permissions:

  ```bash
  chmod 600 /u01/ajd_wallet/ojdbc.properties
  ```
* [ ] Copy required JARs into:

  ```
  $YCSB_HOME/lib/
  ```

---

##  Optional Workload Runs

Once setup is done, you can run:

```bash
# Load data
./bin/ycsb load jdbc -s -P jdbc/oracle-ajd.properties -P workloads/workloada

# Read/update benchmark
./bin/ycsb run jdbc -s -P jdbc/oracle-ajd.properties -P workloads/workloadb
```

---

