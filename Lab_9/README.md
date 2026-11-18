<!-- markdownlint-disable -->

<div align="center">

# 🐘 Experiment 9: Develop MapReduce Application Using Hadoop Cluster Setup

![Header Wave](https://capsule-render.vercel.app/api?type=waving&color=0:090d16,35:0284c7,70:6366f1,100:090d16&height=180&section=header&text=EXPERIMENT%209&fontSize=36&fontColor=ffffff&fontAlignY=38&desc=Develop%20MapReduce%20Application%20Using%20Hadoop%20Cluster%20Setup&descAlignY=60)

[![Course](https://img.shields.io/badge/Course-CSE12045-0284c7?style=flat-square&logo=googlecloud&logoColor=white)](https://adamasuniversity.ac.in/)
[![Hadoop](https://img.shields.io/badge/Big%20Data-Apache%20Hadoop%203.5.0-66CCFF?style=flat-square&logo=apachehadoop&logoColor=black)](https://hadoop.apache.org/)
[![Storage](https://img.shields.io/badge/Storage-HDFS-0284c7?style=flat-square&logo=apachehadoop&logoColor=white)](https://hadoop.apache.org/)
[![Compute](https://img.shields.io/badge/Engine-MapReduce%20%2F%20YARN-f59e0b?style=flat-square)](https://hadoop.apache.org/)
[![Java](https://img.shields.io/badge/Runtime-OpenJDK%2017-E95420?style=flat-square&logo=openjdk&logoColor=white)](https://openjdk.org/)
[![PDF Report](https://img.shields.io/badge/Report-Experiment__9.pdf-red?style=flat-square&logo=adobeacrobatreader&logoColor=white)](Experiment_9.pdf)
[![Back to Root](https://img.shields.io/badge/Back%20to-Main%20README-10b981?style=flat-square&logo=github&logoColor=white)](../README.md)

</div>

---

## 🎯 1. Objective

To deploy and configure an **Apache Hadoop 3.5.0** pseudo-distributed cluster in an Ubuntu Linux environment, configure the **Hadoop Distributed File System (HDFS)** and **YARN** resource manager, establish passwordless SSH communication, format the NameNode, and develop, compile, execute, and verify a distributed **MapReduce WordCount** application processing large text corpora.

---

## 🏗️ 2. Hadoop Pseudo-Distributed Cluster Architecture

```text
               +--------------------------------------------+
               |         Ubuntu Linux Virtual Machine       |
               +---------------------+----------------------+
                                     |
                       +-------------+-------------+
                       |                           |
            +----------+----------+     +----------+----------+
            |      Storage        |     |       Compute       |
            |   (HDFS Daemons)    |     |   (YARN Daemons)    |
            +----------+----------+     +----------+----------+
            |  • NameNode         |     |  • ResourceManager  |
            |  • DataNode         |     |  • NodeManager      |
            |  • SecondaryNN      |     |                     |
            +----------+----------+     +----------+----------+
                       |                           |
                       +-------------+-------------+
                                     |
                      [MapReduce WordCount Job Execution]
```

| Component | Role / Configuration | Ports / Paths |
| :--- | :--- | :--- |
| **Apache Hadoop** | Version 3.5.0 Binary Distribution | Installed at `/usr/local/hadoop` |
| **Java Runtime** | OpenJDK 17 (Java 17.0.21) | `JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64` |
| **HDFS NameNode** | Master node managing filesystem namespace | `hdfs://localhost:9000` |
| **HDFS DataNode** | Worker node storing replicated data blocks | Local disk storage backend |
| **YARN ResourceManager** | Cluster resource scheduling & arbitration | Web UI at `http://localhost:8088` |
| **MapReduce Application** | Custom Java WordCount (Mapper + Reducer) | Packaged as `wordcount.jar` |

---

## 🛠️ 3. Step-by-Step Procedure & Commands

### Step 3.1 — Download & Verify Apache Hadoop 3.5.0
```bash
# Update Ubuntu environment
sudo apt update && sudo apt upgrade -y
sudo apt install -y openjdk-17-jdk openssh-server wget tar

# Download Hadoop 3.5.0 archive & SHA-512 checksum
wget https://dlcdn.apache.org/hadoop/common/stable/hadoop-3.5.0.tar.gz
wget https://dlcdn.apache.org/hadoop/common/stable/hadoop-3.5.0.tar.gz.sha512

# Verify archive integrity and extract
sha512sum -c hadoop-3.5.0.tar.gz.sha512
tar -xzf hadoop-3.5.0.tar.gz
sudo mv hadoop-3.5.0 /usr/local/hadoop
```

### Step 3.2 — Configure Environment & XML Configurations
Add Hadoop environment variables to `~/.bashrc`:
```bash
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export HADOOP_HOME=/usr/local/hadoop
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
source ~/.bashrc
```

Configure `core-site.xml`, `hdfs-site.xml`, `mapred-site.xml`, and `yarn-site.xml` under `$HADOOP_HOME/etc/hadoop`.

### Step 3.3 — Configure Passwordless SSH & Initialize NameNode
```bash
# Enable SSH service
sudo systemctl enable --now ssh

# Generate Ed25519 key pair without passphrase
ssh-keygen -t ed25519 -N "" -f ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
ssh localhost

# Format HDFS NameNode
hdfs namenode -format
```

### Step 3.4 — Launch Hadoop Cluster Daemons
```bash
# Start HDFS (NameNode, DataNode, SecondaryNameNode)
start-dfs.sh

# Start YARN (ResourceManager, NodeManager)
start-yarn.sh

# Verify running Java processes
jps
```

### Step 3.5 — Develop & Run MapReduce Application
```bash
# Create HDFS input directory and upload text dataset
hdfs dfs -mkdir -p /user/student/cloud-lab9/input
hdfs dfs -put input.txt /user/student/cloud-lab9/input/

# Compile WordCount Java source with Hadoop classpath
javac -classpath $(hadoop classpath) -d . WordCount.java
jar -cvf wordcount.jar *.class

# Submit MapReduce Job to YARN
hadoop jar wordcount.jar WordCount /user/student/cloud-lab9/input /user/student/cloud-lab9/output

# View generated WordCount results
hdfs dfs -cat /user/student/cloud-lab9/output/part-r-00000

# Stop Hadoop cluster daemons
stop-yarn.sh
stop-dfs.sh
```

---

## 📊 4. Result & Analysis

- **Cluster Initialization:** Successfully deployed a pseudo-distributed Hadoop 3.5.0 cluster with all HDFS and YARN daemons operational.
- **Distributed Computing:** Submitted the MapReduce WordCount job; observed parallel split mapping, shuffle-and-sort, and reducer word aggregation.
- **HDFS Output:** Confirmed output generation in `/user/student/cloud-lab9/output/part-r-00000` with 100% computational accuracy.
- **Full Report:** Configuration files, daemon verification logs, and job completion traces are documented in [Experiment_9.pdf](Experiment_9.pdf).

---

<div align="center">

[⬅️ Back to Main Repository](../README.md)

</div>
