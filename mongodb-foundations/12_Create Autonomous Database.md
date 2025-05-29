


#  **Step-by-Step: Create Oracle Autonomous JSON Database (AJD) — Always Free**

---

##  **Step 1: Log in to Oracle Cloud Console**

Go to: [https://cloud.oracle.com/](https://cloud.oracle.com/)
Sign in.

---

##  **Step 2: Open Autonomous Database Creation**

Menu → **Oracle Database** → **Autonomous Database** → **Create Autonomous Database**


![image](https://github.com/user-attachments/assets/81293ea0-04fa-4ec3-8322-27af6c8bfca3)
---

##  **Step 3: Fill in the Details**

| Field               | Your Input                                                     |
| ------------------- | -------------------------------------------------------------- |
| **Compartment**     | Choose your working compartment                                |
| **Display Name**    | mongodb-ajd-poc (or any name you like)                         |
| **Database Name**   | auto-filled or custom                                          |
| **Workload Type**   | **JSON**                                                       |
| **Deployment Type** | **Serverless**                                                 |
| **OCPU / Storage**  | Select Always Free (this option will use 1 OCPU + up to 20 GB) |


![image](https://github.com/user-attachments/assets/4a5bee71-2fde-4653-a3d5-2732b7a94ccf)


![image](https://github.com/user-attachments/assets/e69110cb-e106-48ed-8728-9dd7a30a9592)

![image](https://github.com/user-attachments/assets/1d198856-e8eb-44a3-be32-408e8a1216b7)


 **Tip:** If Always Free is greyed out, it means your tenancy limit has been reached — in that case, proceed with paid tier (smallest size).

---

##  **Step 4: Network Access**

Select:

```plaintext
Secure access from everywhere
Allow users with database credentials to access the database from the internet.
```

 This is required since your MongoDB lives in Azure and will connect over the internet.


![image](https://github.com/user-attachments/assets/8f4043a0-7389-4c37-9039-51119ec7d8a3)



![image](https://github.com/user-attachments/assets/8a3e5020-a2cf-4788-a944-66afde731258)


---

##  **Step 5: Auto Scaling**

(Optional but recommended) → **Enable auto scaling**

Even though Always Free has limits, this keeps you ready if you upgrade later.

---

##  **Step 6: License**

Select:
**License included** (unless you have BYOL).

---

##  **Step 7: Tags (optional)**

You can skip this.

---

##  **Step 8: Create Database**

Click **Create Autonomous Database**.

Wait **2–5 minutes** → Status will become **Available**.

---

##  **At this stage: AJD is created.**

**Next step → Enable MongoDB API.**

---

