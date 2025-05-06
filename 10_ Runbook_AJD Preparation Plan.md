# Runbook: Prepare Oracle AJD for MongoDB Migration

![image](https://github.com/user-attachments/assets/408093a6-8f2c-4abe-9c21-6902e5831735)


MongoDB environment is ready
Explore the two migration options
We’ve chosen the right approach for the POC (mongoexport/mongoimport)

---

## Next Step → **AJD Preparation Plan**

Here’s what we’ll do now:

### **Step 2: Prepare Oracle AJD**

| Task                                         | Status |
| -------------------------------------------- | ------ |
| 1️ Provision Autonomous JSON Database (AJD) | WIP    |
| 2️ Enable MongoDB API                       | WIP    |
| 3️ Download Wallet / Connection String      | WIP     |
| 4️ Test connection (mongosh / Compass)      | WIP    |

---

Perfect!
Here’s the clean and professional step-by-step guide for your **Oracle Autonomous JSON Database (AJD)** setup.

---

#  **Runbook: Prepare Oracle AJD for MongoDB Migration**

---

##  **Step 1: Log in to Oracle Cloud Console**

* Go to: [https://cloud.oracle.com/](https://cloud.oracle.com/)
* Login with your credentials.

---

##  **Step 2: Create an Autonomous JSON Database (AJD)**

> **Navigation:**
>
> **Oracle Cloud Console** → **Menu** → **Oracle Database** → **Autonomous Database**

 Click **Create Autonomous Database**.

Fill in the fields:

| Field                | Value                                                                |
| -------------------- | -------------------------------------------------------------------- |
| **Compartment**      | Select your compartment                                              |
| **Display Name**     | `mongodb-ajd-poc` or something meaningful                            |
| **Database Name**    | auto-generated or you can name it                                    |
| **Workload Type**    | Select **JSON**                                                      |
| **Deployment Type**  | Choose **Serverless** (simpler)                                      |
| **OCPU and Storage** | 1 OCPU, 1TB Storage (you can adjust later)                           |
| **License Type**     | BYOL if you have, or **License Included**                            |
| **Auto Scaling**     | Enable it                                                            |
| **Always Free**      | If available in your tenancy and region, you can opt for it for test |

 Click **Create Autonomous Database**.

**After 2–5 minutes**, AJD will be **Provisioned and Available**.

---

##  **Step 3: Enable MongoDB API on AJD**

**After AJD is created:**

* Click on your Autonomous Database (`mongodb-ajd-poc`).
* Go to **"Database Actions"**.
* Under **JSON Database Settings**, **Enable MongoDB API**.

 MongoDB API will be enabled within 1–2 minutes.

---

##  **Step 4: Download Wallet (TLS/SSL Connectivity)**

* Inside your AJD screen, click on **DB Connection**.
* Click **Download Wallet**.
* Save the **zip file** safely (example: `wallet_mongodb-ajd-poc.zip`).

 This wallet allows your **mongosh**, **Compass**, or **App** to securely connect to AJD.

---

##  **Step 5: Test MongoDB API Connectivity**

Now, test whether AJD MongoDB API is reachable.

**Command (local or from your Azure VM):**

```bash
mongosh "mongodb+srv://<mongo-connection-string>" --tlsCAFile /path/to/wallet/ca.pem
```

(Oracle will give you the exact **MongoDB connection string** after enabling MongoDB API.)

 If you see:

```plaintext
Successfully connected to Oracle Autonomous JSON Database via MongoDB API
```

you are ready for import.

---

#  **Summary — After these steps**

| Item                 | Status |
| -------------------- | ------ |
| AJD Database created |       |
| MongoDB API enabled  |       |
| Wallet downloaded    |       |
| Connectivity tested  |       |

Then we are ready for Step 3:
mongoimport to bring your `customers`, `products`, `orders` into AJD.

---


