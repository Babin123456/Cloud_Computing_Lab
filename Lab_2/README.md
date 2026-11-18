<!-- markdownlint-disable -->

<div align="center">

# 💬 Experiment 2: Creation and Network Configuration of Virtual Machines

![Header Wave](https://capsule-render.vercel.app/api?type=waving&color=0:090d16,35:0284c7,70:6366f1,100:090d16&height=180&section=header&text=EXPERIMENT%202&fontSize=36&fontColor=ffffff&fontAlignY=38&desc=Creation%20and%20Network%20Configuration%20of%20Virtual%20Machines&descAlignY=60)

[![Course](https://img.shields.io/badge/Course-CSE12045-0284c7?style=flat-square&logo=googlecloud&logoColor=white)](https://adamasuniversity.ac.in/)
[![Hypervisor](https://img.shields.io/badge/Hypervisor-Oracle%20VirtualBox-2563eb?style=flat-square&logo=virtualbox&logoColor=white)](https://www.virtualbox.org/)
[![Network Mode](https://img.shields.io/badge/Network-NAT%20Network-0284c7?style=flat-square&logo=cisco&logoColor=white)](https://www.virtualbox.org/)
[![PDF Report](https://img.shields.io/badge/Report-Experiment__2.pdf-red?style=flat-square&logo=adobeacrobatreader&logoColor=white)](Experiment_2.pdf)
[![Back to Root](https://img.shields.io/badge/Back%20to-Main%20README-10b981?style=flat-square&logo=github&logoColor=white)](../README.md)

</div>

---

## 🎯 1. Objective

To create a second Ubuntu virtual machine (`ubuntu-client (clone)`) by duplicating an existing Ubuntu server VM using VirtualBox's **Full Clone** mechanism, configure an isolated **NAT Network** (`Dummy`) with DHCP enabled, verify assigned dynamic IP configurations across both virtual endpoints, and demonstrate bidirectional host-to-host ICMP connectivity.

---

## 🌐 2. Virtual Network Topology & Addressing

```text
                  +-------------------------------------+
                  |   VirtualBox NAT Network: "Dummy"   |
                  |         Subnet: 192.168.50.0/24     |
                  |         DHCP Server: Enabled        |
                  +----------+---------------+----------+
                             |               |
                             |               |
              +--------------+--+         +--+--------------+
              |  ubuntu-server  |         |  ubuntu-client  |
              |     (Primary)   |         |     (Clone)     |
              |  192.168.50.3   |         |  192.168.50.4   |
              +-----------------+         +-----------------+
                             <--- Ping (0% Loss) --->
```

| Parameter | Server VM (`ubuntu-server`) | Client VM (`ubuntu-client`) |
| :--- | :--- | :--- |
| **VM Nature** | Primary Master Virtual Machine | Full Clone (Independent disk) |
| **Network Adapter** | Adapter 1: NAT Network (`Dummy`) | Adapter 1: NAT Network (`Dummy`) |
| **MAC Address Policy** | Original Hardware Address | Reinitialized Unique MAC Address |
| **Assigned IPv4 Address** | `192.168.50.3 / 24` | `192.168.50.4 / 24` |
| **Gateway Address** | `192.168.50.1` | `192.168.50.1` |

---

## 🛠️ 3. Step-by-Step Procedure

### Step 3.1 — Create VirtualBox NAT Network
1. Navigate to **VirtualBox Manager** -> **Tools** -> **Network Manager** -> **NAT Networks**.
2. Click **Create** and name the network `Dummy`.
3. Set IPv4 Prefix to `192.168.50.0/24` and ensure **Enable DHCP** is checked.

### Step 3.2 — Clone the Ubuntu Server VM
1. Right-click `ubuntu-server` and select **Clone**.
2. Name the clone `ubuntu-client (clone)`.
3. Choose **Full Clone** and select **Generate new MAC addresses for all network adapters**.

### Step 3.3 — Attach Virtual Network Adapters
1. Open VM Settings for both virtual machines.
2. Under **Network** -> **Adapter 1**, set *Attached to:* **NAT Network**.
3. Select *Name:* **Dummy** and click **OK**.

### Step 3.4 — Verify Network Interfaces & Connectivity
Start both VMs, open terminals, and run:

```bash
# Check assigned IP addresses on each machine
ip a

# Test ping from Server VM (192.168.50.3) to Client VM (192.168.50.4)
ping 192.168.50.4

# Test ping from Client VM (192.168.50.4) to Server VM (192.168.50.3)
ping 192.168.50.3
```

---

## 📊 4. Result & Verification

- **Full Clone Integrity:** Created a fully independent client virtual machine with distinct MAC and IP assignments.
- **Two-Way Reachability:** Achieved **0% packet loss** across multiple ICMP echo requests between endpoints on the `192.168.50.0/24` subnet.
- **Full Report:** Complete configuration screenshots, adapter settings, and terminal logs are available in [Experiment_2.pdf](Experiment_2.pdf).

---

<div align="center">

[⬅️ Back to Main Repository](../README.md)

</div>
