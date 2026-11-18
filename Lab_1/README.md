<!-- markdownlint-disable -->

<div align="center">

# 📦 Experiment 1: Installation of Hypervisors and Initiation of VMs

![Header Wave](https://capsule-render.vercel.app/api?type=waving&color=0:090d16,35:0284c7,70:6366f1,100:090d16&height=180&section=header&text=EXPERIMENT%201&fontSize=36&fontColor=ffffff&fontAlignY=38&desc=Installation%20of%20Hypervisors%20and%20Initiation%20of%20VMs&descAlignY=60)

[![Course](https://img.shields.io/badge/Course-CSE12045-0284c7?style=flat-square&logo=googlecloud&logoColor=white)](https://adamasuniversity.ac.in/)
[![Hypervisor](https://img.shields.io/badge/Hypervisor-Oracle%20VirtualBox-2563eb?style=flat-square&logo=virtualbox&logoColor=white)](https://www.virtualbox.org/)
[![Guest OS](https://img.shields.io/badge/Guest%20OS-Ubuntu%20Linux-E95420?style=flat-square&logo=ubuntu&logoColor=white)](https://ubuntu.com/)
[![PDF Report](https://img.shields.io/badge/Report-Experiment__1.pdf-red?style=flat-square&logo=adobeacrobatreader&logoColor=white)](Experiment_1.pdf)
[![Back to Root](https://img.shields.io/badge/Back%20to-Main%20README-10b981?style=flat-square&logo=github&logoColor=white)](../README.md)

</div>

---

## 🎯 1. Objective

To install and configure an open-source hypervisor (**Oracle VirtualBox**), instantiate an Ubuntu Virtual Machine (`ubuntu-server`) using an ISO image, configure the virtual hardware parameters, execute unattended guest OS installation, boot the guest operating system, verify kernel execution, and install essential build tools and Linux kernel headers.

---

## ⚙️ 2. Hardware Sizing & Virtual Environment

| Resource / Parameter | Specification | Details / Purpose |
| :--- | :--- | :--- |
| **Hypervisor** | **Oracle VirtualBox** | Type-2 Bare-Metal Virtualization Emulator |
| **Virtual Machine Name** | `ubuntu-server` | Dedicated host for cloud lab workloads |
| **Guest OS Image** | `ubuntu-24.04-live-server` | 64-bit Linux Server ISO distribution |
| **Base Memory (RAM)** | **5120 MB (5 GB)** | Allocated guest memory |
| **Processors (vCPUs)** | **3 vCPUs** | Execution cores assigned to guest |
| **Virtual Storage** | **25.00 GB** | VDI (VirtualBox Disk Image), dynamically allocated |
| **Firmware Mode** | BIOS / EFI | Unattended installation enabled |

---

## 🛠️ 3. Step-by-Step Procedure

### Step 3.1 — Create Virtual Machine in VirtualBox
1. Launch **Oracle VirtualBox** and select **New**.
2. Set VM Name to `ubuntu-server`, select Type `Linux`, Version `Ubuntu (64-bit)`.
3. Attach the downloaded official Ubuntu Live Server ISO image.
4. Enable unattended guest OS installation with credentials and hostname.

### Step 3.2 — Configure Hardware Resources
1. Allocate **5120 MB RAM** and **3 vCPUs**.
2. Create a virtual hard disk of **25.00 GB**.
3. Finish configuration wizard and start the VM.

### Step 3.3 — Boot & Verify Linux Kernel
1. Allow the automated installer to partition disks, initialize root filesystems, and install base packages.
2. Log into the Ubuntu bash terminal environment.
3. Verify kernel architecture and release:

```bash
uname -a
```

### Step 3.4 — Install Build Essentials & Kernel Headers
Synchronize local repositories and install essential compilation toolchains and matching kernel headers:

```bash
# Update repository indexes
sudo apt update

# Install build tools, GCC, make, and kernel headers
sudo apt install -y build-essential linux-headers-$(uname -r)

# Verify GCC compilation toolchain
gcc --version
```

---

## 📊 4. Result & Verification

- **VM Provisioning:** Successfully created and instantiated the `ubuntu-server` guest VM under Oracle VirtualBox.
- **Kernel Headers:** Verified active Linux kernel release and compiled module readiness via installed `linux-headers` and `build-essential`.
- **Full Report:** Terminal screenshots, configuration screens, and step-by-step documentation are available in [Experiment_1.pdf](Experiment_1.pdf).

---

<div align="center">

[⬅️ Back to Main Repository](../README.md)

</div>
