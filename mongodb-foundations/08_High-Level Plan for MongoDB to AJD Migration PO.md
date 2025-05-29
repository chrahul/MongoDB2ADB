
---

#  **High-Level Plan for MongoDB → AJD Migration POC**

Think of this in **5 Phases**:

---

##  **Phase 1: Assessment & Planning (What to move)**

| Task                    | Outcome                                        |
| ----------------------- | ---------------------------------------------- |
| Review collections      | Already have customers, products, orders |
| Understand data size    |  \~75 MB to 100 MB total                      |
| Confirm data model      |  JSON documents                               |
| Decide migration method |  mongoexport/mongoimport for POC             |

**Goal:** Know *what* and *how much* you’re migrating.

---

##  **Phase 2: Prepare Oracle AJD (Where to move)**

| Task                                     | Outcome       |
| ---------------------------------------- | ------------- |
| Provision Autonomous JSON Database (AJD) |  To be done |
| Enable MongoDB API                       |  To be done |
| Download Wallet / Connection String      |  To be done |
| Test connection (mongosh / Compass)      |  To be done |

**Goal:** Have the *target* database ready and connectable.

---

##  **Phase 3: Export Data from MongoDB**

| Task                                      | Outcome                                       |
| ----------------------------------------- | --------------------------------------------- |
| Use mongoexport to export each collection |  customers.json, products.json, orders.json |
| Validate JSON files                       |  Check for completeness                     |

**Goal:** Get data out of MongoDB in a portable format.

---

##  **Phase 4: Import Data into AJD**

| Task                                          | Outcome                             |
| --------------------------------------------- | ----------------------------------- |
| Use mongoimport with AJD Mongo API connection |  Load customers, products, orders |
| Verify document counts                        |  Validate                         |
| Run sample queries                            |  Validate structure               |

**Goal:** Load data into AJD and verify.

---

##  **Phase 5: Performance & Compatibility Testing**

| Task                                  | Outcome                           |
| ------------------------------------- | --------------------------------- |
| Test queries via mongosh, Compass     | 🔜 Compare to MongoDB performance |
| Test app-level queries (if needed)    | 🔜                                |
| Optional: Enable Oracle SQL over JSON | 🔜                                |

**Goal:** Ensure the migrated data works for real queries.

---

#  **POC Success Criteria**

| Goal                                | Status |
| ----------------------------------- | ------ |
| Data fully exported & imported      | WIP     |
| Structure and document counts match | WIP     |
| Queries work as expected            | WIP     |
| Performance acceptable              | WIP     |

---

## **Summary**

**Your MongoDB side is 100% ready.**
Now we start:

1. Export collections
2. Prepare AJD
3. Import collections
4. Test queries

---

