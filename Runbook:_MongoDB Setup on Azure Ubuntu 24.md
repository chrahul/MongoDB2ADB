# **complete Runbook** 

* Install MongoDB
* Install Python and libraries
* Create data generator script
* Populate the database

Not just MongoDB setup — but **end to end**:
**MongoDB + Database + Collections + Data Generation**

---

#  **Runbook: Complete MongoDB Lab Setup with Real Data (Azure Ubuntu 24.04)**

##  **Environment**

* Azure Ubuntu VM 24.04 LTS
* User with `sudo` privileges
* Public access or VPN for MongoDB if needed

---

##  **Step 1: Update the OS**

```bash
sudo apt update
sudo apt upgrade -y
```

---

##  **Step 2: Install Required Tools**

```bash
sudo apt install gnupg curl -y
```

---

##  **Step 3: Add MongoDB GPG Key**

```bash
curl -fsSL https://pgp.mongodb.com/server-7.0.asc | sudo gpg --dearmor -o /usr/share/keyrings/mongodb-server-7.0.gpg
```

---

##  **Step 4: Add MongoDB Repository**

```bash
echo "deb [ arch=amd64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

---

##  **Step 5: Install MongoDB**

```bash
sudo apt update
sudo apt install -y mongodb-org
```

---

##  **Step 6: Start MongoDB and Enable Auto-start**

```bash
sudo systemctl start mongod
sudo systemctl enable mongod
```

**Check Status:**

```bash
sudo systemctl status mongod
```

 You should see **Active (running)**.

---

##  **Step 7: Test MongoDB Shell**

```bash
mongosh
```

**Switch to the database:**

```javascript
use salesdb
```

 At this point, MongoDB is running but **no data yet**.

---

##  **Step 8: Install Python Virtual Environment**

```bash
sudo apt install python3-venv -y
```

 Check Python:

```bash
python3 --version
```

---

##  **Step 9: Create Python Virtual Environment**

```bash
python3 -m venv mymongoenv
```

**Activate it:**

```bash
source mymongoenv/bin/activate
```

Your shell should now show:

```bash
(mymongoenv) kumar@MongoDB:~$
```

---

##  **Step 10: Install Python Libraries (Inside Virtual Env)**

```bash
pip install pymongo faker tqdm
```

 Libraries installed:

* **pymongo** → MongoDB driver
* **faker** → Fake data generator
* **tqdm** → Progress bar for scripts

---

##  **Step 11: Create Python Data Generator Script**

```bash
nano generate_sales_data.py
```

**Paste the script** (this is exactly the version you used with the date fix applied):

```python
from pymongo import MongoClient
from faker import Faker
from tqdm import tqdm
import random

client = MongoClient("mongodb://localhost:27017/")
db = client["salesdb"]

customers_col = db["customers"]
orders_col = db["orders"]
products_col = db["products"]

faker = Faker()

NUM_CUSTOMERS = 100000
NUM_PRODUCTS = 1000
NUM_ORDERS = 500000

customers_col.drop()
orders_col.drop()
products_col.drop()

print("Generating Customers...")
customers = []
for _ in tqdm(range(NUM_CUSTOMERS)):
    customers.append({
        "name": faker.name(),
        "email": faker.email(),
        "address": faker.address(),
        "phone": faker.phone_number()
    })
customers_col.insert_many(customers)

print("Generating Products...")
products = []
for _ in tqdm(range(NUM_PRODUCTS)):
    products.append({
        "product_name": faker.word(),
        "price": round(random.uniform(5.0, 500.0), 2),
        "category": faker.word()
    })
products_col.insert_many(products)

print("Generating Orders...")
orders = []
for _ in tqdm(range(NUM_ORDERS)):
    orders.append({
        "customer_id": random.randint(1, NUM_CUSTOMERS),
        "product_id": random.randint(1, NUM_PRODUCTS),
        "quantity": random.randint(1, 10),
        "total_price": round(random.uniform(10.0, 1000.0), 2),
        "order_date": faker.date_time_this_year().isoformat()
    })

batch_size = 10000
for i in tqdm(range(0, len(orders), batch_size)):
    orders_col.insert_many(orders[i:i + batch_size])

print("Data generation complete!")
```

Save and exit:
CTRL+O → Enter → CTRL+X

---

##  **Step 12: Run the Script to Populate Data**

```bash
python3 generate_sales_data.py
```

 Script will:

* Create **100,000 customers**
* Create **1,000 products**
* Create **500,000 orders**
* Load data into `salesdb` MongoDB

You will see progress bars and finally:

```plaintext
Data generation complete!
```

---

##  **Step 13: Verify Data in MongoDB**

```bash
mongosh
```

```javascript
use salesdb
show collections
db.customers.count()
db.products.count()
db.orders.count()
```

You should see:

```plaintext
100000
1000
500000
```

 Your database is now ready for testing and migration.

---

##  **Final Setup Summary**

| Component                     | Status |
| ----------------------------- | ------ |
| MongoDB Installed             | Done      |
| MongoDB Running               | Done     |
| Python Virtual Env Created    | Done      |
| Libraries Installed           | Done      |
| Data Generator Script Created | Done      |
| Database Populated            | Done      |

---


---


