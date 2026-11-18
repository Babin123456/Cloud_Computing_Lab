<!-- markdownlint-disable -->

<div align="center">

# ⚙️ Experiment 5: Implementation of Various Scheduling Mechanisms

![Header Wave](https://capsule-render.vercel.app/api?type=waving&color=0:090d16,35:0284c7,70:6366f1,100:090d16&height=180&section=header&text=EXPERIMENT%205&fontSize=36&fontColor=ffffff&fontAlignY=38&desc=Implementation%20of%20Various%20Scheduling%20Mechanisms&descAlignY=60)

[![Course](https://img.shields.io/badge/Course-CSE12045-0284c7?style=flat-square&logo=googlecloud&logoColor=white)](https://adamasuniversity.ac.in/)
[![Simulator](https://img.shields.io/badge/Simulator-CloudSim%20Plus%208.5.5-f59e0b?style=flat-square&logo=apachemaven&logoColor=white)](https://cloudsimplus.org/)
[![Java](https://img.shields.io/badge/Runtime-OpenJDK%2017-E95420?style=flat-square&logo=openjdk&logoColor=white)](https://openjdk.org/)
[![Build](https://img.shields.io/badge/Build-Maven%203.9-C71A36?style=flat-square&logo=apachemaven&logoColor=white)](https://maven.apache.org/)
[![PDF Report](https://img.shields.io/badge/Report-Experiment__5.pdf-red?style=flat-square&logo=adobeacrobatreader&logoColor=white)](Experiment_5.pdf)
[![Back to Root](https://img.shields.io/badge/Back%20to-Main%20README-10b981?style=flat-square&logo=github&logoColor=white)](../README.md)

</div>

---

## 🎯 1. Objective

To implement, evaluate, and compare different **Virtual Machine (VM)** and **Cloudlet (Task)** scheduling mechanisms using the **CloudSim Plus 8.5.5** open-source cloud simulation framework. The experiment models a virtualized cloud datacenter, provisions computing resources, submits workloads, and analyzes execution behaviors under all four combinations of **SpaceShared** and **TimeShared** scheduling policies.

---

## 🔬 2. Scheduling Policies & Resource Configuration

```text
               +--------------------------------------------+
               |               Cloud Datacenter             |
               |       Host: 4 PEs (1000 MIPS), 8GB RAM     |
               +---------------------+----------------------+
                                     |
                       +-------------+-------------+
                       |                           |
                 +-----+-----+               +-----+-----+
                 |   VM 0    |               |   VM 1    |
                 | 2 PEs, 2GB|               | 2 PEs, 2GB|
                 +-----+-----+               +-----+-----+
                       |                           |
                   [Cloudlets]                 [Cloudlets]
```

### Scheduling Policy Matrix
| Scenario | VM Scheduler Policy | Cloudlet Scheduler Policy | Total Tasks | Completion Status |
| :---: | :---: | :---: | :---: | :---: |
| **A** | `VmSchedulerSpaceShared` | `CloudletSchedulerSpaceShared` | 4 | **4 / 4 SUCCESS** |
| **B** | `VmSchedulerSpaceShared` | `CloudletSchedulerTimeShared` | 4 | **4 / 4 SUCCESS** |
| **C** | `VmSchedulerTimeShared` | `CloudletSchedulerSpaceShared` | 4 | **4 / 4 SUCCESS** |
| **D** | `VmSchedulerTimeShared` | `CloudletSchedulerTimeShared` | 4 | **4 / 4 SUCCESS** |

---

## 🛠️ 3. Step-by-Step Procedure & Commands

### Step 3.1 — Install Java 17, Maven & Git
```bash
sudo apt update
sudo apt upgrade -y
sudo apt install openjdk-17-jdk maven git -y

# Verify toolchains
java -version
mvn -version
```

### Step 3.2 — Initialize Maven Project & Dependencies
Create directory hierarchy:

```bash
mkdir -p ~/CloudSchedulingLab5/src/main/java/lab
cd ~/CloudSchedulingLab5
```

Configure `pom.xml` with CloudSim Plus 8.5.5 and Exec Maven Plugin:

```xml
<dependencies>
    <dependency>
        <groupId>org.cloudsimplus</groupId>
        <artifactId>cloudsimplus</artifactId>
        <version>8.5.5</version>
    </dependency>
</dependencies>
```

Resolve dependencies:

```bash
mvn dependency:resolve
```

### Step 3.3 — Implement Simulation (`SchedulingExperiment.java`)
Create `src/main/java/lab/SchedulingExperiment.java` modeling Datacenter, Hosts, VMs, Cloudlets, and executing Scenarios A, B, C, and D.

### Step 3.4 — Compile & Execute Simulation
```bash
# Compile project
mvn clean compile

# Execute main simulation class
mvn org.codehaus.mojo:exec-maven-plugin:3.5.0:java -Dexec.mainClass="lab.SchedulingExperiment"
```

---

## 📊 4. Result & Analysis

- **Execution Metrics:** All 4 Cloudlets across Scenarios A through D achieved status **SUCCESS** (start time `0.10s`, finish time `20.10s`).
- **Policy Comparison:** SpaceShared grants exclusive hardware utilization, whereas TimeShared distributes processing capacity concurrently over simulation clock cycles.
- **Full Report:** Code listings, Maven build logs, and console output tables are available in [Experiment_5.pdf](Experiment_5.pdf).

---

<div align="center">

[⬅️ Back to Main Repository](../README.md)

</div>
