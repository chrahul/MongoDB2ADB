**Stage 1: MongoDB Baseline on Azure**. 
Below is the **step-by-step implementation guide**. Each step includes commands, configuration hints, and best practices.

---

#  **Stage 1 – MongoDB Baseline on Azure**

---

##  1. Provision Azure VM

###  VM Specs

* **VM Type:** D4s v5
* **OS:** Ubuntu 22.04 LTS
* **Size:** 4 vCPU, 16 GB RAM
* **Disk:** Premium SSD (100 GB+)
* **Public IP:** Enable (for dev only)
* **VNet/Subnet:** Default or custom

###  Azure CLI Setup

```bash
# Login
az login

# Create Resource Group
az group create --name mongoLabRG --location eastus

# Create VM
az vm create \
  --resource-group mongoLabRG \
  --name mongoAppVM \
  --image Ubuntu2204 \
  --size Standard_D4s_v5 \
  --admin-username azureuser \
  --generate-ssh-keys \
  --storage-sku Premium_LRS
```

---

##  2. Install & Configure MongoDB 6.x (Standalone Replica Set)

###  MongoDB Install

```bash
# Add repo key and source
curl -fsSL https://pgp.mongodb.com/server-6.0.asc | sudo gpg -o /usr/share/keyrings/mongodb-server-6.0.gpg --dearmor
echo "deb [ signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list

# Install MongoDB
sudo apt update
sudo apt install -y mongodb-org

# Enable MongoDB
sudo systemctl start mongod
sudo systemctl enable mongod
```

###  Enable Replica Set (even standalone)

```bash
# Edit config
sudo nano /etc/mongod.conf

# Add under replication
replication:
  replSetName: "rs0"

# Restart and initiate replica set
sudo systemctl restart mongod
mongosh
> rs.initiate()
```

---

##  3. Deploy Sample MERN App using Docker Compose

###  Recommended App

GitHub: [sample-mflix](https://github.com/PranavKonduru/sample-mflix-mern)

###  Folder Structure

```bash
mkdir ~/mongo-benchmark-lab && cd ~/mongo-benchmark-lab
git clone https://github.com/PranavKonduru/sample-mflix-mern app
cd app
```

###  Docker Compose

Create `docker-compose.yml`:

```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - mongo
    environment:
      - MONGO_URI=mongodb://mongo:27017/mflix

  mongo:
    image: mongo:6.0
    container_name: mflix-db
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db
    command: --replSet rs0

volumes:
  mongo-data:
```

```bash
# Build and launch
docker-compose up -d

# Init replica set inside container
docker exec -it mflix-db mongosh
> rs.initiate()
```

---

##  4. Seed Data (\~5 GB) & Warm Cache

###  Download Dataset

```bash
wget https://media.mongodb.org/datasets/mflix/mflix.tar.gz
tar -xzf mflix.tar.gz
```

###  Import Data

```bash
docker cp mflix/ mflix-db:/data/mflix/
docker exec -it mflix-db mongosh
> use mflix
> db.movies.insertMany([...])  # You can write import scripts for each collection
```

Or use `mongoimport`:

```bash
docker exec -it mflix-db bash
mongoimport --db mflix --collection movies --file /data/mflix/movies.json --jsonArray
```

###  Warm-up

Run the app and browse/search for 15–20 mins OR use dummy scripts that randomly fetch & insert documents.

---

##  5. Generate Load Using k6

###  Install k6

```bash
docker run -it --rm -v $(pwd):/scripts grafana/k6 run /scripts/loadtest.js
```

###  Sample `loadtest.js`

```js
import http from 'k6/http';
import { check, sleep } from 'k6';

export let options = {
  vus: 500,
  duration: '10m'
};

export default function () {
  let res = http.get('http://<public_ip>:3000/api/movies');
  check(res, { 'status was 200': (r) => r.status === 200 });
  sleep(1);
}
```

Replace endpoint if needed with API that hits Mongo.

---

##  6. Capture Metrics

###  MongoDB Metrics

```bash
docker exec -it mflix-db bash
mongostat --rowcount 600 --discover > mongostat.csv
mongotop -n 5 > mongotop.txt
```

###  System Metrics

Install and run node exporter:

```bash
docker run -d -p 9100:9100 --name=node-exporter prom/node-exporter
```

Monitor via Prometheus or use `htop`, `vmstat`, `iotop` for snapshots.

---

##  7. Export Results

Organize results:

```bash
mkdir ~/mongo-benchmark-results
cp mongostat.csv ~/mongo-benchmark-results/
cp loadtest-summary.json ~/mongo-benchmark-results/
```

Optionally:

* Export Azure Monitor dashboard as workbook
* Zip `/var/lib/docker/volumes/mongo-data` for storage snapshots

---


