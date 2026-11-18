<!-- markdownlint-disable -->

<div align="center">

# 🌐 Experiment 3: Installation and Basic Operation of Mininet

![Header Wave](https://capsule-render.vercel.app/api?type=waving&color=0:090d16,35:0284c7,70:6366f1,100:090d16&height=180&section=header&text=EXPERIMENT%203&fontSize=36&fontColor=ffffff&fontAlignY=38&desc=Installation%20and%20Basic%20Operation%20of%20Mininet&descAlignY=60)

[![Course](https://img.shields.io/badge/Course-CSE12045-0284c7?style=flat-square&logo=googlecloud&logoColor=white)](https://adamasuniversity.ac.in/)
[![Emulator](https://img.shields.io/badge/Emulator-Mininet%202.3.0-14b8a6?style=flat-square&logo=gnubash&logoColor=white)](http://mininet.org/)
[![Switch](https://img.shields.io/badge/Switch-Open%20vSwitch-2563eb?style=flat-square&logo=linux&logoColor=white)](https://www.openvswitch.org/)
[![PDF Report](https://img.shields.io/badge/Report-Experiment__3.pdf-red?style=flat-square&logo=adobeacrobatreader&logoColor=white)](Experiment_3.pdf)
[![Back to Root](https://img.shields.io/badge/Back%20to-Main%20README-10b981?style=flat-square&logo=github&logoColor=white)](../README.md)

</div>

---

## 🎯 1. Objective

To install and verify **Mininet 2.3.0** inside an Ubuntu Linux virtual machine, confirm system package integrity, execute the built-in self-test, instantiate parameterized Software-Defined Networking (SDN) topologies (Single-switch and Hierarchical Tree topologies), and evaluate end-to-end host connectivity using the interactive Mininet CLI.

---

## 🏗️ 2. Topologies Evaluated

```text
1. Single Switch Topology (single,3):
                   +---------+
                   | Switch  |
                   |   (s1)  |
                   +----+----+
                        |
             +----------+----------+
             |          |          |
           +---+      +---+      +---+
           |h1 |      |h2 |      |h3 |
           +---+      +---+      +---+

2. Hierarchical Tree Topology (tree, depth=3):
   • 7 OpenFlow Switches (s1 - s7)
   • 8 Virtual End-Hosts (h1 - h8)
```

| Topology Scenario | Switches | Hosts | Total Ping Paths | Packet Loss Observed |
| :--- | :---: | :---: | :---: | :---: |
| **Built-in Self Test** | 1 (`s1`) | 2 (`h1`, `h2`) | 2 | **0% dropped** (2/2 received) |
| **Single Switch (`single,3`)** | 1 (`s1`) | 3 (`h1`–`h3`) | 6 | **0% dropped** (6/6 received) |
| **Tree Topology (`tree,3`)** | 7 (`s1`–`s7`) | 8 (`h1`–`h8`) | 56 | **0% dropped** (56/56 received) |

---

## 🛠️ 3. Step-by-Step Procedure & Commands

### Step 3.1 — Install & Verify Mininet
Open Ubuntu terminal and install Mininet from official apt repositories:

```bash
# Update repository catalog
sudo apt update

# Install Mininet
sudo apt install mininet -y

# Verify installed release
mn --version
```

### Step 3.2 — Execute Built-in Self-Test
Run Mininet's default ping test verifying Open vSwitch kernel module initialization:

```bash
sudo mn --test pingall
```

### Step 3.3 — Instantiate Single-Switch Topology
Create a topology connecting 3 hosts to 1 switch and test reachability:

```bash
sudo mn --topo single,3
```

Inside the `mininet>` prompt, execute:

```text
mininet> nodes
mininet> links
mininet> pingall
mininet> exit
```

### Step 3.4 — Instantiate Depth-3 Tree Topology
Create a multi-tiered switch topology:

```bash
sudo mn --topo tree,3
```

Inside the `mininet>` prompt:

```text
mininet> pingall
mininet> exit
```

### Step 3.5 — Clean Virtual Network State
Flush residual Open vSwitch ports, virtual ethernet pairs, and daemon processes:

```bash
sudo mn -c
```

---

## 📊 4. Result & Verification

- **Package Verification:** Confirmed active **Mininet 2.3.0** on Linux kernel.
- **Mesh Connectivity:** Verified 100% reachability (0% packet drop) across basic pair tests, 3-host single-switch setups, and complex 8-host/7-switch tree topologies.
- **Full Report:** Terminal command outputs, link states, and ping matrices are documented in [Experiment_3.pdf](Experiment_3.pdf).

---

<div align="center">

[⬅️ Back to Main Repository](../README.md)

</div>
