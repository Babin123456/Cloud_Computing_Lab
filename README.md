<!-- markdownlint-disable MD033 MD041 -->

<div align="center">

# 💻 Cloud Computing Laboratory (CSE12045) 💻

![Cloud Computing Lab Header](https://capsule-render.vercel.app/api?type=waving&color=0:090d16,35:0284c7,70:6366f1,100:090d16&height=200&section=header&text=CLOUD%20COMPUTING%20LAB&fontSize=38&fontColor=ffffff&fontAlignY=36&desc=(CSE12045)%20%E2%80%A2%207th%20Semester&descAlignY=58)

[![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=18&duration=3000&pause=1000&color=38BDF8&center=true&vCenter=true&width=700&height=40&lines=%E2%98%81%EF%B8%8F+Cloud+Computing+Laboratory+(CSE12045);%F0%9F%8E%93+ADAMAS+University+%E2%80%A2+Department+of+CSE;%F0%9F%91%A9%E2%80%8D%F0%9F%8F%AB+Under+the+Guidance+of+Prof.+Anusuya+Bera+Ma'am;%F0%9F%91%A8%E2%80%8D%F0%9F%92%BB+Performed+by+Babin+Bid+(UG%2F02%2FBTCSE%2F2023%2F114))](https://git.io/typing-svg)

[![Course](https://img.shields.io/badge/Course-CSE12045-0284c7?style=flat-square&logo=googlecloud&logoColor=white)](https://adamasuniversity.ac.in/)
[![Semester](https://img.shields.io/badge/Semester-7th%20Sem-8b5cf6?style=flat-square)](https://adamasuniversity.ac.in/)
[![University](https://img.shields.io/badge/University-ADAMAS%20University-f97316?style=flat-square)](https://adamasuniversity.ac.in/)
[![Stream](https://img.shields.io/badge/Stream-B.Tech%20CSE%20(Core)-10b981?style=flat-square)](https://adamasuniversity.ac.in/)
[![Hypervisor](https://img.shields.io/badge/Hypervisor-Oracle%20VirtualBox-2563eb?style=flat-square&logo=virtualbox&logoColor=white)](https://www.virtualbox.org/)
[![OS](https://img.shields.io/badge/Guest%20OS-Ubuntu%20Linux-E95420?style=flat-square&logo=ubuntu&logoColor=white)](https://ubuntu.com/)
[![SDN](https://img.shields.io/badge/Emulation-Mininet%202.3.0-14b8a6?style=flat-square&logo=gnubash&logoColor=white)](http://mininet.org/)
[![License](https://img.shields.io/badge/License-MIT-22c55e?style=flat-square)](LICENSE)

</div>

---

<div align="center">

## 🧑‍💻 Student & Academic Details 🧑‍💻

| Attribute | Details |
| :---: | :---: |
| 🧑‍🎓 **Student Name** | **Babin Bid** |
| 🔢 **Roll Number** | `UG/02/BTCSE/2023/114` |
| 🆔 **Registration Number** | `AU/2023/0009748` |
| 🏷️ **Section & Stream** | **Section B** \| **B.Tech CSE (Core)** |
| 📖 **Course & Semester** | **Cloud Computing Lab (`CSE12045`)** • **7th Semester** |
| 👩‍🏫 **Course Instructor** | **Prof. Anusuya Bera Ma'am** (Dept. of CSE) |
| 🏛️ **Institution** | **ADAMAS University**, Kolkata |

</div>

---

<div align="center">

## 🖥️ Laboratory Experiments 🖥️

| # | Experiment Title | Key Tools & Tech | Documentation Report |
| :---: | :--- | :--- | :---: |
| **01** | **Installation of Hypervisors and Initiation of VMs** | VirtualBox, Ubuntu ISO (`ubuntu-server`), GCC, Kernel Headers | [Experiment_1.pdf](Experiment_1.pdf) |
| **02** | **Creation and Network Configuration of Virtual Machines** | VirtualBox Full Clone, NAT Network (`Dummy`), ICMP Ping | [Experiment_2.pdf](Experiment_2.pdf) |
| **03** | **Installation and Basic Operation of Mininet** | Ubuntu VM, Mininet 2.3.0, Open vSwitch, Single & Tree Topologies | [Experiment_3.pdf](Experiment_3.pdf) |

</div>

---

## 📦 Experiment 1: Installation of Hypervisors and Initiation of VMs

- **Objective:** Install and configure an open-source hypervisor (**Oracle VirtualBox**), instantiate an Ubuntu Virtual Machine (`ubuntu-server`) using an ISO image, configure virtual hardware, boot the guest OS, verify execution, and install essential build tools and Linux kernel headers.
- **Hardware Sizing:**
  - **Memory:** 5120 MB RAM (5 GB)
  - **Processors:** 3 vCPUs
  - **Virtual Storage:** 25.00 GB virtual disk
- **Running Commands & Syntaxes:**

  ```bash
  # Step 1: Update local package repository index
  sudo apt update

  # Step 2: Install build-essential and Linux kernel headers for hypervisor module support
  sudo apt install -y build-essential linux-headers-$(uname -r)

  # Step 3: Verify kernel and architecture
  uname -a
  ```

- **Report & Output:** Successfully created and initialized the `ubuntu-server` VM with kernel headers verified. Complete step-by-step setup and terminal screenshots are documented in [Experiment_1.pdf](Experiment_1.pdf).

---

## 💬 Experiment 2: Creation and Network Configuration of Virtual Machines

- **Objective:** Create a second Ubuntu virtual machine (`ubuntu-client (clone)`) by duplicating the server VM via Full Clone, place both VMs on an isolated VirtualBox NAT Network (`Dummy`), verify assigned IP addresses, and test two-way communication using the `ping` command.
- **Virtual Network Specifications:**
  - **Network Mode:** NAT Network
  - **Network Name:** `Dummy`
  - **Subnet:** `192.168.50.0/24` with DHCP enabled
  - **Server VM Endpoint:** `192.168.50.3`
  - **Client VM Endpoint:** `192.168.50.4`
- **Running Commands & Syntaxes:**

  ```bash
  # Step 1: Check assigned IP addresses and network interfaces on both VMs
  ip a

  # Step 2: Test connectivity from Ubuntu Server VM to Client VM (Target: 192.168.50.4)
  ping 192.168.50.4

  # Step 3: Test connectivity from Ubuntu Client VM to Server VM (Target: 192.168.50.3)
  ping 192.168.50.3
  ```

- **Report & Output:** Achieved 0% packet loss in both directions across the `192.168.50.0/24` virtual network. Full clone dialogs, adapter attachments, and terminal ping outputs are documented in [Experiment_2.pdf](Experiment_2.pdf).

---

## 🌐 Experiment 3: Installation and Basic Operation of Mininet

- **Objective:** Install and verify **Mininet 2.3.0** inside the Ubuntu virtual machine, execute the built-in ping test, instantiate simple built-in topologies (Single Switch and Tree topologies), and evaluate host-to-host connectivity using the Mininet CLI `pingall` command.
- **Topologies Evaluated:**
  - **Built-in Ping Test:** 2 virtual hosts (`h1`, `h2`) connected to switch `s1` (`0% dropped, 2/2 received`).
  - **Single-Switch Topology with 3 Hosts:** 1 switch (`s1`) and 3 hosts (`h1`, `h2`, `h3`) (`0% dropped, 6/6 received`).
  - **Tree Topology (Depth 3):** 7 switches (`s1`–`s7`) and 8 hosts (`h1`–`h8`) (`0% dropped, 56/56 received`).
- **Running Commands & Syntaxes:**

  ```bash
  # Step 1: Update package list and install Mininet
  sudo apt update
  sudo apt install mininet -y

  # Step 2: Verify installed Mininet version (Mininet 2.3.0)
  mn --version

  # Step 3: Run the built-in pingall self-test
  sudo mn --test pingall

  # Step 4: Create a single-switch topology with 3 hosts
  sudo mn --topo single,3

  # Inside Mininet CLI for single-switch topology:
  mininet> links
  mininet> pingall
  mininet> exit

  # Step 5: Create a hierarchical tree topology with depth 3
  sudo mn --topo tree,3

  # Inside Mininet CLI for tree topology:
  mininet> pingall
  mininet> exit

  # Step 6: Clean up Mininet virtual network state and residual daemons
  sudo mn -c
  ```

- **Report & Output:** Verified Mininet 2.3.0 with complete reachability (0% packet drop) across single and depth-3 tree SDN topologies. Complete command outputs are documented in [Experiment_3.pdf](Experiment_3.pdf).

---

## 📜 Academic Notice & Coursework Disclaimer

This repository and its contents are developed and maintained by **Babin Bid** (Roll No: `UG/02/BTCSE/2023/114`, Reg No: `AU/2023/0009748`) for academic coursework and laboratory evaluation in the **Cloud Computing Lab (CSE12045)** under the Department of Computer Science & Engineering, School of Engineering & Technology, **ADAMAS University**, supervised by **Prof. Anusuya Bera**.

The materials, configuration steps, and documentation provided herein are intended strictly for educational, experimental, and academic research purposes.

Licensed under the [MIT License](LICENSE).

---

![Footer Wave Banner](https://capsule-render.vercel.app/api?type=waving&color=0:6366f1,50:0284c7,100:090d16&height=100&section=footer)

<div align="center">

*Developed by **Babin Bid** • ADAMAS University*

</div>
