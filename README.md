<div align="center">

<!-- Animated Header Wave Banner -->
<img src="https://capsule-render.vercel.app/api?type=waving&color=0:090d16,35:0284c7,70:6366f1,100:090d16&height=200&section=header&text=CLOUD%20COMPUTING%20LAB&fontSize=38&fontColor=ffffff&fontAlignY=36&desc=(CSE12045)%20%E2%80%A2%207th%20Semester&descAlignY=58" width="100%" alt="Cloud Computing Lab Header" />

<!-- Dynamic Animated Typewriter Effect -->
<a href="https://git.io/typing-svg">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=18&duration=3000&pause=1000&color=38BDF8&center=true&vCenter=true&width=700&height=40&lines=%E2%98%81%EF%B8%8F+Cloud+Computing+Laboratory+(CSE12045);%F0%9F%8E%93+ADAMAS+University+%E2%80%A2+Department+of+CSE;%F0%9F%91%A9%E2%80%8D%F0%9F%8F%AB+Under+the+Guidance+of+Prof.+Anusuya+Bera+Ma'am;%F0%9F%91%A8%E2%80%8D%F0%9F%92%BB+Performed+by+Babin+Bid+(UG%2F02%2FBTCSE%2F2023%2F114)" alt="Typing SVG" />
</a>

<br/>

[![Course](https://img.shields.io/badge/Course-CSE12045-0284c7?style=flat-square&logo=googlecloud&logoColor=white)](https://adamasuniversity.ac.in/)
[![Semester](https://img.shields.io/badge/Semester-7th%20Sem-8b5cf6?style=flat-square)](https://adamasuniversity.ac.in/)
[![University](https://img.shields.io/badge/University-ADAMAS%20University-f97316?style=flat-square)](https://adamasuniversity.ac.in/)
[![Stream](https://img.shields.io/badge/Stream-B.Tech%20CSE%20(Core)-10b981?style=flat-square)](https://adamasuniversity.ac.in/)
[![License](https://img.shields.io/badge/License-MIT-22c55e?style=flat-square)](LICENSE)

</div>

---

### 📌 Student & Academic Details

| Attribute | Details |
| :--- | :--- |
| 🧑‍🎓 **Student Name** | **Babin Bid** |
| 🔢 **Roll Number** | `UG/02/BTCSE/2023/114` |
| 🆔 **Registration Number** | `AU/2023/0009748` |
| 🏷️ **Section & Stream** | **Section B** \| **B.Tech CSE (Core)** |
| 📖 **Course & Semester** | **Cloud Computing Lab (`CSE12045`)** • **7th Semester** |
| 👩‍🏫 **Course Instructor** | **Prof. Anusuya Bera Ma'am** (Dept. of CSE) |
| 🏛️ **Institution** | **ADAMAS University**, Kolkata |

---

### 🔬 Laboratory Experiments

| # | Experiment Title | Key Tools | Report File |
| :-: | :--- | :--- | :-: |
| **01** | **Installation of Hypervisors and initiation of VMs with image file.** | VirtualBox, Ubuntu ISO, GCC, Kernel Headers | `Lab 1.pdf` |
| **02** | **Client–Server Communication Between Two Virtual Machines** | VirtualBox NAT Network, Python TCP Sockets | `Lab 2.pdf` |
| **03** | **Creation of a Simple Network Topology Using Open-Source Network Virtualization Tools** | Ubuntu VM, Mininet, Open vSwitch | `Lab Experiment 3.pdf` |

---

### 📦 Experiment 1: Hypervisor & Virtual Machine Initiation
- **Objective:** Install and configure an open-source hypervisor (**Oracle VirtualBox**) and instantiate a **Virtual Machine (VM)** using an Ubuntu Linux ISO image.
- **Key Tasks Performed:**
  - Configured VM hardware: 2 vCPUs, 4 GB RAM, 25 GB VDI storage, NAT adapter.
  - Installed guest OS and verified uptime and CPU virtualization (`uname -a`, `lscpu`).
  - Installed required build tools and Linux kernel headers:
    ```bash
    sudo apt update && sudo apt install -y build-essential linux-headers-$(uname -r)
    ```
  - Captured relevant configuration and execution screenshots in the lab report.

---

### 💬 Experiment 2: Client–Server Communication Between Two VMs
- **Objective:** Configure two Virtual Machines on a shared virtual network in VirtualBox to establish client–server connectivity and run a TCP-based chat application.
- **Key Tasks Performed:**
  - Configured both VMs under a shared **NAT Network** (`Dummy`).
  - Verified assigned IP addresses (`ip a`) and ICMP connectivity (`ping <Server_IP> -c 4`).
  - Designated **VM 1 as Server** (listening on port `65432`) and **VM 2 as Client**.
  - Implemented and demonstrated an interactive TCP socket chat application in Python.
  - Captured screenshots of network configuration, ping tests, and active chat exchange.

---

### 🌐 Experiment 3: Simple Network Topology Using Mininet
- **Objective:** Create and configure a simple network topology using an open-source network virtualization tool (**Mininet**) and verify node communication.
- **Key Tasks Performed:**
  - Deployed Mininet inside the Ubuntu Linux Virtual Machine.
  - Created simple network topologies using Mininet CLI and Python scripts:
    ```bash
    sudo mn --topo single,3
    ```
  - Tested node reachability and bandwidth using `pingall`, `iperf`, `net`, and `dump`.
  - Cleaned up the virtual network state after execution:
    ```bash
    sudo mn -c
    ```
  - Captured network topology and connectivity verification screenshots.

---

### 📜 Academic Notice & Coursework Disclaimer

This repository and its contents are developed and maintained by **Babin Bid** (Roll No: `UG/02/BTCSE/2023/114`, Reg No: `AU/2023/0009748`) for academic coursework and laboratory evaluation in the **Cloud Computing Lab (CSE12045)** under the Department of Computer Science & Engineering, School of Engineering & Technology, **ADAMAS University**, supervised by **Prof. Anusuya Bera**.

The materials, source codes, configuration scripts, and documentation provided herein are intended strictly for educational, experimental, and academic research purposes.

Licensed under the **[MIT License](LICENSE)**.

<div align="center">

<!-- Bottom Animated Waving Capsule -->
<img src="https://capsule-render.vercel.app/api?type=waving&color=0:6366f1,50:0284c7,100:090d16&height=100&section=footer" width="100%" alt="Footer Wave Banner" />

<sub>Developed by <b>Babin Bid</b> • ADAMAS University</sub>

</div>
