
# 📘 ELK Cluster Installation Guide (AWS EC2 + Docker)

This document provides step-by-step instructions to deploy an Elasticsearch (ELK) cluster with 1 master + 2 data nodes + Kibana on AWS EC2 using Docker.

---

## ✅ 1. AWS Security Group Configuration

Create a new security group and add the following inbound rules:

| Type        | Protocol | Port Range | Source                  | Purpose                     |
|-------------|----------|-------------|--------------------------|-----------------------------|
| SSH         | TCP      | 22          | Your IP                 | SSH Access                  |
| Custom TCP  | TCP      | 9200        | Your IP                 | Elasticsearch REST API      |
| Custom TCP  | TCP      | 5601        | Your IP                 | Kibana Web Access           |
| Custom TCP  | TCP      | 9200-9300   | Security group (self)   | Node-to-node communication  |
| All ICMP - IPv4 | ICMP | All         | Security group (self)   | ICMP (ping) traffic         |

> Outbound rules are open to the world by default (0.0.0.0/0).

---

## 🛠️ 2. Installing Docker on EC2

### ▶ EC2 Setup:
- OS: Ubuntu 22.04
- Public IP: Yes (Auto-assign Public IP = Enable)
- Must be in the same VPC and subnet

### ▶ Connect via SSH:
```bash
ssh -i <your-key.pem> ubuntu@<EC2_PUBLIC_IP>
```

### ▶ Install Docker and Compose:
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install docker.io docker-compose -y
sudo usermod -aG docker $USER
newgrp docker
```

### ▶ System Settings:
```bash
echo "vm.max_map_count=262144" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

---

## 🌐 3. Elasticsearch Master Node Setup

### 📁 `docker-compose.yml` (on EC2 Master Node)

```yaml
version: '3.8'
services:
  es-master:
    image: docker.elastic.co/elasticsearch/elasticsearch:8.12.1
    container_name: es-master
    environment:
      - node.name=es-master
      - cluster.name=elk-cluster
      - node.roles=master
      - discovery.seed_hosts=10.0.41.228,10.0.33.242
      - cluster.initial_master_nodes=es-master
      - network.host=0.0.0.0
      - network.publish_host=10.0.65.222
      - xpack.security.enabled=false
    ports:
      - "9200:9200"
      - "9300:9300"
    volumes:
      - es-master-data:/usr/share/elasticsearch/data
    networks:
      - elk

  kibana:
    image: docker.elastic.co/kibana/kibana:8.12.1
    container_name: kibana
    ports:
      - "5601:5601"
    environment:
      - ELASTICSEARCH_HOSTS=http://10.0.65.222:9200
      - xpack.security.enabled=false
    depends_on:
      - es-master
    networks:
      - elk

volumes:
  es-master-data:

networks:
  elk:
```

### ▶ Start the Containers:
```bash
docker-compose up -d
```

### ▶ Test:
```bash
curl http://localhost:9200
```

---

## 🚗 4. Elasticsearch Data Node Setup

After installing Docker on each data node EC2, follow these steps:

### 📁 `docker-compose-data1.yml` (Data Node 1 - IP: X.X.X.X)


### 📁 `docker-compose-data2.yml` (Data Node 2 - IP: X.X.X.X )


### 📁 `docker-compose-data3.yml` (Data Node 3 - PRIVATE IP: X.X.X.X )



### ▶ Start the Data Node Containers:
```bash
docker-compose up -d
```

---

## 🔍 Validate Cluster Status

### ✔ List of Nodes:
```bash
curl http://localhost:9200/_cat/nodes?v
```

### ✔ Shard Distribution:
```bash
curl http://localhost:9200/_cat/shards?v
```

### ✔ Cluster health:
```bash
curl http://localhost:9200/_cluster/health?pretty
```

---

## 🌟 Conclusion

With this setup, you now have a fully functional 3-node Elasticsearch cluster (1 master, 2 data nodes).

You can now integrate Spark, Logstash, Beats, APM, or begin ingesting data.

> ✔ Kibana Access: `http://<master-node-public-ip>:5601`

---
