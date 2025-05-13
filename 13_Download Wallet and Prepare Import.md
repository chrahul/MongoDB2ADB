**AJD is ready** and **MongoDB API is visible**


---

## A Quick Recap: Where we Are

| Task                                                           | Status |
| ---------------------------------------------------------------- | ------ |
| MongoDB on Azure with sample data (\~600K docs)                  | Done |
| mongoexport of 3 collections (`customers`, `products`, `orders`) | Done |
| Oracle AJD (JSON workload) provisioned                           | Done |
| MongoDB API visible and enabled in AJD console                   | Done |

---

##  **Next Step: Download Wallet and Prepare Import**

###  **1️ Download the Database Wallet**

From the AJD console:

* Click **“DB Connection”**
* Choose **“Download Wallet”**
* Save and unzip it on the same machine where you will run `mongoimport`
  (e.g., your Azure VM)

```bash
unzip wallet_mongoDb2AJD.zip -d wallet_dir
```

You’ll use `ca.pem` from this folder to make a secure connection.

---

###  **2️ Get the MongoDB API Connection String**

After enabling the API, Oracle provides a **MongoDB-compatible URI**, something like:

```
mongodb+srv://admin@your-db-name.mongodb.region.oraclecloud.com
```

Use this with `mongosh`, `mongoimport`, and MongoDB Compass.

---

###  **3️ Test MongoDB API Connectivity**

From your Azure VM (or any client machine with Mongo tools):

```bash
mongosh "mongodb+srv://..." \
--tlsCAFile /full/path/to/wallet_dir/ca.pem
```

You should see:

```bash
> Successfully connected
```

 This confirms that **AJD is reachable** via MongoDB protocol.

---

###  **4️ Import Data via mongoimport**

Now import your exported collections one by one:

```bash
mongoimport --uri "mongodb+srv://..." \
--collection customers \
--file customers.json \
--tls \
--tlsCAFile /full/path/to/wallet_dir/ca.pem
```

Repeat for `products.json` and `orders.json`.

---

###  **5️ Validate the Data**

Use `mongosh` to run:

```javascript
use salesdb
show collections
db.customers.count()
db.products.count()
db.orders.count()
```

 Confirm document counts match your original MongoDB database.

---

##  Optional: Create Indexes

You can create indexes post-import to improve query speed:

```javascript
db.orders.createIndex({ customer_id: 1 })
```

---


