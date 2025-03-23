# Apache Spark 3 Pseudo-Cluster Installation on Ubuntu 20.04  
**Single-Node Setup for Data Scientists and Developers**

This guide provides step-by-step instructions to set up Apache Spark 3 in **pseudo-cluster mode** (one-node setup) on **Ubuntu 20.04**. It covers how to configure Spark with both Python (PySpark) and Scala environments.

---

## Table of Contents

- [Prerequisites](#prerequisites)  
- [Step 1 – Install Java](#step-1--install-java)  
- [Step 2 – Install Apache Spark](#step-2--install-apache-spark)  
- [Step 3 – Configure Environment Variables](#step-3--configure-environment-variables)  
- [Step 4 – Start Spark in Pseudo-Cluster Mode](#step-4--start-spark-in-pseudo-cluster-mode)  
- [Step 5 – Using Spark with Python (PySpark)](#step-5--using-spark-with-python-pyspark)  
- [Step 6 – Using Spark with Scala](#step-6--using-spark-with-scala)  
- [Tips and Notes](#tips-and-notes)

---

## Prerequisites

- Operating System: Ubuntu 20.04 LTS  
- sudo privileges  
- Internet access  

---

### Install Java 11
  
[Java Installation Guide](https://github.com/Furkaragoz/big-data-ai-installation/tree/main/java))


---

## Step 2 – Install Apache Spark

### Download Spark
```bash
wget https://dlcdn.apache.org/spark/spark-3.2.1/spark-3.2.1-bin-hadoop3.2.tgz
```

### Extract the Archive
```bash
tar xvf spark-3.2.1-bin-hadoop3.2.tgz
```

### Move Spark to /opt Directory
```bash
sudo mv spark-3.2.1-bin-hadoop3.2 /opt/spark
```

---

## Step 3 – Configure Environment Variables

### Edit `.bashrc`
```bash
nano ~/.bashrc
```

### Append the following lines:
```bash
# Spark Environment
export SPARK_HOME=/opt/spark
export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin
export PYSPARK_PYTHON=/usr/bin/python3
```

### Reload `.bashrc`
```bash
source ~/.bashrc
```

---

## Step 4 – Start Spark in Pseudo-Cluster Mode

### Start Spark Master and Worker
```bash
cd /opt/spark
./sbin/start-master.sh
./sbin/start-slave.sh spark://<YOUR-HOSTNAME>:7077
```

> Replace `<YOUR-HOSTNAME>` with the actual hostname or IP address.

### Access Spark Web UI

Open your browser and visit:
```
http://<YOUR-IP-ADDRESS>:8080
```

---

## Step 5 – Using Spark with Python (PySpark)

### Launch PySpark Shell
```bash
pyspark
```

### Example: Word Count with PySpark

#### 1. Download a Sample Text File
```bash
wget -O alice.txt https://www.gutenberg.org/files/11/11-0.txt
```

#### 2. Run in PySpark Shell
```python
input = sc.textFile("/home/ubuntu/alice.txt")
words = input.flatMap(lambda line: line.split())
wordCounts = words.countByValue()

for word, count in wordCounts.items():
    print(f"{word} {count}")
```

---

## Step 6 – Using Spark with Scala

### Launch Scala Spark Shell
```bash
spark-shell
```

You can now write Spark applications in Scala interactively.

---

## Tips and Notes

- Spark Master UI: [http://localhost:8080](http://localhost:8080)
- Log files are located under: `/opt/spark/logs`
- Use `pyspark` for Python-based development and `spark-shell` for Scala.
- Ideal for development, testing, and educational purposes.
- For distributed setups, consider deploying Spark with YARN, Kubernetes, or on cloud platforms.

---

Feel free to contribute or suggest improvements via pull requests or issues.

---

Let me know if you'd like to add:
- Hadoop HDFS integration  
- `spark-submit` usage examples  
- Jupyter Notebook support for PySpark  
- Dockerfile for containerized setup  
- Multi-node cluster instructions
