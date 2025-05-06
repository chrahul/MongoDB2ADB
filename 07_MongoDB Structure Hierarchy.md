![image](https://github.com/user-attachments/assets/22477a2b-bf26-4670-a947-6e0e61bbfa5e)



# Read this with RUNBOOK

---

##  **MongoDB Structure Hierarchy**

```plaintext
MongoDB Instance (mongod running on port 27017)
│
├── Database(s) (Example: salesdb)
│   │
│   ├── Collection(s) (Example: customers, orders, products)
│   │   │
│   │   ├── Document(s) (Example: individual customer, order, or product)
│   │
│   └── Index(es)
│
└── Users, Roles, etc.
```

**Your setup right now**:

* **MongoDB Instance** → Running on **localhost:27017** (your Azure VM)
* **Database** → `salesdb`
* **Collections** → `customers`, `orders`, `products`
* **Documents** → \~100,000 customers, 500,000 orders, 1,000 products

---

##  **What is a Database (`salesdb`)**

In MongoDB:

* A **database** (`salesdb`) is a container for collections.
* Like a **schema** in Oracle, but more flexible.

**Example:**

```plaintext
salesdb
 ├── customers
 ├── orders
 └── products
```

You can have multiple databases in MongoDB:

```plaintext
admin
config
local
salesdb
```

But in your case, you have **salesdb** as the main database.

---

##  **What is a Collection**

A **collection** is like a **table** in relational databases.

* Collections **store documents**.
* Collections don’t enforce schemas (*you can have flexible document structures*).

Your collections:

* **customers** → 100,000 documents (each = 1 customer)
* **orders** → 500,000 documents (each = 1 order)
* **products** → 1,000 documents (each = 1 product)

---

##  **What is a Document**

A **document** is a **record** (in JSON format).

**Example customer document:**

```json
{
  "name": "Rahul",
  "email": "rahul@example.com",
  "address": "123 MG Road, Bangalore",
  "phone": "9999999999"
}
```

**Example order document:**

```json
{
  "customer_id": 68449,
  "product_id": 310,
  "quantity": 6,
  "total_price": 441.29,
  "order_date": "2025-03-07"
}
```

Each document can have different fields. That’s why MongoDB is called **schemaless** or **polymorphic**.

---

##  **Your db.stats() Explained**

```json
db: 'salesdb',
collections: Long('3'),
views: Long('0'),
objects: Long('601000'),   // Total documents (customers + orders + products)
avgObjSize: 125.07 bytes,
dataSize: 75171259 bytes (~75 MB),
storageSize: 31137792 bytes (~31 MB),
indexes: Long('3'),
indexSize: 6242304 bytes (~6 MB),
totalSize: 37380096 bytes (~37 MB)
```

| Field         | Meaning                                                    |
| ------------- | ---------------------------------------------------------- |
| `db`          | Database name (salesdb)                                    |
| `collections` | Number of collections (3)                                  |
| `objects`     | Total number of documents across all collections (601,000) |
| `avgObjSize`  | Average size per document (125 bytes)                      |
| `dataSize`    | Raw data size (\~75 MB)                                    |
| `storageSize` | Physical space used (\~31 MB)                              |
| `indexSize`   | Size of indexes (\~6 MB)                                   |

---

##  **Why is the size smaller than 10GB?**

Your earlier Faker-generated data had random values → compact JSON.
The test dataset is \~75 MB now. (The size depends on the content, and this was an efficient data structure.)

If you want, we can also:

* Increase **order document count** or
* Add **longer fields (more data)** to bulk up the size toward 10GB.

---

##  Quick Summary

| Term               | Meaning                                           |
| ------------------ | ------------------------------------------------- |
| Database (salesdb) | Main container for collections                    |
| Collections        | Groups of documents (customers, orders, products) |
| Documents          | Individual records (JSON objects)                 |

---

