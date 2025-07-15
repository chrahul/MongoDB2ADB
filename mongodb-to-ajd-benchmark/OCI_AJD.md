Benchmarking Oracle Autonomous JSON Database (AJD) using YCSB – Final Step-by-Step Guide

Introduction
This document provides a complete step-by-step setup for benchmarking Oracle Autonomous JSON Database (AJD) using Yahoo! Cloud Serving Benchmark (YCSB). It includes environment setup, dependencies, configurations, and troubleshooting based on real execution experience.


Step 1: Environment Setup
We used an Oracle Cloud Infrastructure (OCI) CentOS-based VM to execute the benchmarking.
Ensure you have Oracle Cloud CLI and an internet connection enabled. All steps were executed as the `root` user.

Step 2: Install Oracle Instant Client
Download and install the following RPMs from Oracle:
- oracle-instantclient-basic-21.18.0.0.0-1.el8.x86_64.rpm
- oracle-instantclient-jdbc-21.18.0.0.0-1.el8.x86_64.rpm

Then install using:
$ yum -y localinstall oracle-instantclient-*.rpm


Step 3: Clone and Build YCSB
Clone YCSB from GitHub:
$ git clone https://github.com/brianfrankcooper/YCSB.git
$ cd YCSB
Build YCSB JDBC binding:
$ mvn -pl site.ycsb:jdbc-binding -am clean package


Step 4: Add AJD Wallet & JDBC Drivers
Download the AJD wallet and unzip to a local directory (e.g., /root/YCSB/wallet).

Make sure `oraclepki.jar`, `osdt_core.jar`, and `osdt_cert.jar` are included in:

`~/YCSB/jdbc/target/ycsb-jdbc-binding-0.18.0-SNAPSHOT/lib/`
⚙ Step 5: Create Properties File
Create `oracle-ajd.properties` inside the `jdbc/` directory:

```
db.driver=oracle.jdbc.OracleDriver
db.url=jdbc:oracle:thin:@<AJD_TNS_ALIAS>?TNS_ADMIN=/root/YCSB/wallet
db.user=ADMIN
db.passwd=<YOUR_PASSWORD>
jdbc.batchupdateapi=true
recordcount=1000000
operationcount=1000000
```

Replace <AJD_TNS_ALIAS> and <YOUR_PASSWORD> accordingly.

Step 6: Load Data to AJD
Run the following command to load data:
$ ./bin/ycsb load jdbc -P workloads/workloada -P jdbc/oracle-ajd.properties -threads 20 -s -p status.interval=5


Step 7: Run Benchmark
Execute the benchmarking workload using:
$ ./bin/ycsb run jdbc -P workloads/workloada -P jdbc/oracle-ajd.properties -threads 20 -s -p status.interval=5

Common Errors and Fixes

- Missing JARs like `oraclepki.jar`, `osdt_core.jar`: Ensure they are in lib directory and included in classpath.
- TNS_ADMIN not found: Confirm correct wallet path is used in db.url.
- Permissions error: Ensure you have `chmod +r` on wallet files for root user.
- `Missing property: workload`: Always use `-P workloads/workloada` with your run/load commands.

Conclusion
Following this guide ensures a clean and repeatable benchmarking environment for comparing AJD performance. Feel free to fork and enhance this guide in your GitHub repository.
