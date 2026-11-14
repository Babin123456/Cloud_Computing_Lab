<!-- markdownlint-disable -->

# Cloud Computing Lab 4

## Software-Defined Networking (SDN) --- Complete Installation & Experiment Guide

> **Platform:** Ubuntu 26.04 (tested workflow)\
> **Controller:** Ryu 4.34\
> **Python runtime:** Python 3.9\
> **Network emulator:** Mininet\
> **Virtual switch:** Open vSwitch (OVS)\
> **Protocol:** OpenFlow 1.3

------------------------------------------------------------------------

## Table of Contents

1.  [Aim](#1-aim)
2.  [What You Will Build](#2-what-you-will-build)
3.  [Requirements](#3-requirements)
4.  [Before You Begin](#4-before-you-begin)
5.  [Install System Packages](#5-install-system-packages)
6.  [Install uv and Python 3.9](#6-install-uv-and-python-39)
7.  [Create the Ryu Virtual
    Environment](#7-create-the-ryu-virtual-environment)
8.  [Install Ryu 4.34](#8-install-ryu-434)
9.  [Verify the Installation](#9-verify-the-installation)
10. [Run the SDN Experiment](#10-run-the-sdn-experiment)
11. [Inspect OpenFlow Rules](#11-inspect-openflow-rules)
12. [Apply and Test a Filtering
    Rule](#12-apply-and-test-a-filtering-rule)
13. [Remove Rules and Clean Up](#13-remove-rules-and-clean-up)
14. [Troubleshooting](#14-troubleshooting)
15. [Quick Command Reference](#15-quick-command-reference)
16. [Expected Result](#16-expected-result)

------------------------------------------------------------------------

## 1. Aim

To set up a Software-Defined Networking (SDN) environment using
**Mininet**, **Open vSwitch**, and the **Ryu SDN Controller**; create a
network topology with three hosts; test host connectivity; inspect
OpenFlow flow entries; and demonstrate traffic filtering.

## 2. What You Will Build

Software-Defined Networking separates the control plane from the data
plane. In this experiment, Ryu acts as the controller and Open vSwitch
acts as the programmable switch. Mininet creates the virtual hosts and
network topology.

### Network topology

``` text
                 +----------------------+
                 |     Ryu Controller   |
                 |   OpenFlow 1.3       |
                 |     TCP port 6653    |
                 +----------+-----------+
                            |
                     OpenFlow connection
                            |
                 +----------+-----------+
                 |    Open vSwitch      |
                 |         S1           |
                 +----+--------+----+---+
                      |        |    |
                     H1       H2   H3
```

-   **H1, H2, H3:** Mininet virtual hosts.
-   **S1:** Open vSwitch software switch.
-   **Ryu:** Remote SDN controller.
-   **OpenFlow 1.3:** Protocol used for controller-switch communication.

## 3. Requirements

### Software

  Component      Purpose
  -------------- ---------------------------------------------------------
  Ubuntu 26.04   Operating system used in the tested workflow
  Mininet        Creates virtual hosts, switches, and links
  Open vSwitch   Provides the software-based network switch
  Python 3.9     Isolated runtime used for Ryu
  Ryu 4.34       SDN controller
  OpenFlow 1.3   Controller-switch communication protocol
  Git            Retrieves the Ryu source repository
  `uv`           Installs Python 3.9 and manages the virtual environment

### Important compatibility note

The simple `pip install ryu` command in the original assignment did not
work in the tested Ubuntu 26.04 / Python 3.14 environment. The workflow
in this guide therefore uses an isolated Python 3.9 environment and the
Faucet-maintained Ryu repository at tag `v4.34`.

This avoids changing Ubuntu's system Python. The guide records the
installation route that was successfully tested in this environment;
package repositories and dependencies can change, so perform the
verification checks rather than assuming installation succeeded.

## 4. Before You Begin

1.  Start the Ubuntu virtual machine.
2.  Log in with a user account that has `sudo` privileges.
3.  Open Terminal using **Ctrl + Alt + T**.
4.  Keep an internet connection available for package and source
    downloads.

### Terminal convention used in this guide

  Terminal     Responsibility
  ------------ -----------------------------------------------------
  Terminal 1   Runs the Ryu controller; keep it open
  Terminal 2   Runs Mininet and accepts `mininet>` commands
  Terminal 3   Runs Open vSwitch inspection and flow-rule commands

Commands shown with a `$`-style shell context are run in an Ubuntu
terminal. Commands shown with `mininet>` are run inside the Mininet CLI.
Do not enter Mininet commands in the ordinary Ubuntu shell.

------------------------------------------------------------------------

## 5. Install System Packages

### Step 5.1 --- Update Ubuntu

Run package list update first:

``` bash
sudo apt update
```

Then upgrade existing packages:

``` bash
sudo apt upgrade -y
```

### Step 5.2 --- Install required packages

You can install the required packages individually or in single-line statements:

``` bash
sudo apt install -y git
```

``` bash
sudo apt install -y curl
```

``` bash
sudo apt install -y ca-certificates
```

``` bash
sudo apt install -y build-essential
```

``` bash
sudo apt install -y mininet
```

``` bash
sudo apt install -y openvswitch-switch
```

``` bash
sudo apt install -y python3
```

``` bash
sudo apt install -y python3-pip
```

``` bash
sudo apt install -y python3-venv
```

*(Alternatively, all in a single line without line continuations)*:

``` bash
sudo apt install -y git curl ca-certificates build-essential mininet openvswitch-switch python3 python3-pip python3-venv
```

These packages provide Git, download utilities, build tools, Mininet,
Open vSwitch, and the standard Python tools.

### Step 5.3 --- Enable and start Open vSwitch

``` bash
sudo systemctl enable --now openvswitch-switch
```

Check the service:

``` bash
sudo systemctl status openvswitch-switch
```

Press `q` to leave the status screen.

Check that the OVS database responds:

``` bash
sudo ovs-vsctl show
```

**Note:** The service may display `active (exited)` because of how the
service unit is configured. Use `sudo ovs-vsctl show` as an additional
check that Open vSwitch is responding.

### Step 5.4 --- Verify Mininet

``` bash
mn --version
```

If the command is not found or Mininet is not installed correctly,
resolve that before continuing.

------------------------------------------------------------------------

## 6. Install uv and Python 3.9

The tested Ubuntu installation provides a newer system Python. Ryu 4.34
has older packaging and dependency constraints, so this experiment uses
Python 3.9 in an isolated environment.

### Step 6.1 --- Install uv

``` bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Load the environment in the current terminal:

``` bash
source "$HOME/.local/bin/env"
```

Verify:

``` bash
uv --version
```

If `uv` is still not found, open a new terminal and run the `source`
command again.

### Step 6.2 --- Install Python 3.9

``` bash
uv python install 3.9
```

Find the installed interpreter:

``` bash
uv python find 3.9
```

The command should print the path to the Python 3.9 executable managed
by uv.

------------------------------------------------------------------------

## 7. Create the Ryu Virtual Environment

Create an isolated environment named `ryu-env39` in the home directory:

``` bash
uv venv --python 3.9 ~/ryu-env39
```

Activate it:

``` bash
source ~/ryu-env39/bin/activate
```

Verify the Python version:

``` bash
python --version
```

Expected version family:

``` text
Python 3.9.x
```

> **Important:** Keep Ryu and its Python dependencies inside this
> environment. Do not install them into Ubuntu's system Python. In the
> commands below, the explicit `--python ~/ryu-env39/bin/python` option
> ensures that packages are installed into the intended environment.

------------------------------------------------------------------------

## 8. Install Ryu 4.34

### Step 8.1 --- Install compatible build tools

``` bash
uv pip install --python ~/ryu-env39/bin/python setuptools==57.5.0 wheel
```

The pinned setuptools version is part of the tested installation route
and avoids the build-hook issue encountered with newer packaging tools.

### Step 8.2 --- Install Ryu from the Faucet-maintained repository

Execute the single-line command:

``` bash
uv pip install --python ~/ryu-env39/bin/python --no-build-isolation --no-deps "git+https://github.com/faucetsdn/ryu.git@v4.34"
```

This retrieves the `v4.34` source using Git and installs it into the
Python 3.9 environment.

### Step 8.3 --- Install runtime dependencies

Because the Ryu installation above uses `--no-deps`, install the runtime
packages individually or in a single line:

``` bash
uv pip install --python ~/ryu-env39/bin/python eventlet
```

``` bash
uv pip install --python ~/ryu-env39/bin/python msgpack
```

``` bash
uv pip install --python ~/ryu-env39/bin/python netaddr
```

``` bash
uv pip install --python ~/ryu-env39/bin/python oslo.config
```

``` bash
uv pip install --python ~/ryu-env39/bin/python ovs
```

``` bash
uv pip install --python ~/ryu-env39/bin/python routes
```

``` bash
uv pip install --python ~/ryu-env39/bin/python six
```

``` bash
uv pip install --python ~/ryu-env39/bin/python tinyrpc
```

``` bash
uv pip install --python ~/ryu-env39/bin/python webob
```

*(Alternatively, all runtime dependencies together in one single line)*:

``` bash
uv pip install --python ~/ryu-env39/bin/python eventlet msgpack netaddr oslo.config ovs routes six tinyrpc webob
```

------------------------------------------------------------------------

## 9. Verify the Installation

Run both checks from an Ubuntu terminal.

### Step 9.1 --- Verify Ryu version

``` bash
~/ryu-env39/bin/ryu-manager --version
```

Expected:

``` text
ryu-manager 4.34
```

### Step 9.2 --- Verify the OpenFlow 1.3 application

``` bash
~/ryu-env39/bin/python -c "import ryu.app.simple_switch_13; print('OpenFlow 1.3 application OK')"
```

Expected:

``` text
OpenFlow 1.3 application OK
```

If either check fails, do not proceed to the topology stage. Review the
troubleshooting section first.

------------------------------------------------------------------------

## 10. Run the SDN Experiment

### Step 10.1 --- Start the Ryu controller (Terminal 1)

Open the first Ubuntu terminal.

Activate the environment:

``` bash
source ~/ryu-env39/bin/activate
```

Start Ryu with the OpenFlow 1.3 learning-switch application:

``` bash
ryu-manager --ofp-tcp-listen-port 6653 ryu.app.simple_switch_13
```

**Leave Terminal 1 open.** The controller must remain running while
Mininet connects to it.

### Step 10.2 --- Start the Mininet topology (Terminal 2)

Open a second Ubuntu terminal. Run this on a single line:

``` bash
sudo mn --topo single,3 --controller=remote,ip=127.0.0.1,port=6653 --switch ovsk,protocols=OpenFlow13
```

This creates one switch connected to three hosts and configures a remote
controller at `127.0.0.1:6653`.

### Step 10.3 --- Check the nodes

At the `mininet>` prompt:

``` text
nodes
```

The topology should contain:

``` text
h1 h2 h3 s1
```

### Step 10.4 --- Check network connections

``` text
net
```

This displays the host-to-switch connections and switch interfaces. Use
this output to confirm the port connected to H1 before applying the
filtering rule.

### Step 10.5 --- Test connectivity between all hosts

``` text
pingall
```

A successful test should report no packet loss. For three hosts, Mininet
tests six directed host-to-host paths.

Example successful result:

``` text
*** Results: 0% dropped (6/6 received)
```

### Step 10.6 --- Test H1-to-H2 connectivity

``` text
h1 ping h2
```

The command sends continuous ICMP echo requests. Press **Ctrl + C** in
the Mininet terminal to stop the ping after confirming replies.

------------------------------------------------------------------------

## 11. Inspect OpenFlow Rules

Open a third Ubuntu terminal (Terminal 3). Run:

``` bash
sudo ovs-ofctl -O OpenFlow13 dump-flows s1
```

This displays the flow entries installed on switch `s1`.

The output can include controller-installed rules, packet counters,
priorities, match fields, and actions. The exact entries and counters
depend on the state of the experiment.

------------------------------------------------------------------------

## 12. Apply and Test a Filtering Rule

### Step 12.1 --- Confirm H1's switch port

In the Mininet terminal, run:

``` text
net
```

Confirm that H1 is connected to switch port `1`. The filtering command
below assumes that mapping.

### Step 12.2 --- Add the drop rule (Terminal 3)

Run the rule addition command on a single line:

``` bash
sudo ovs-ofctl -O OpenFlow13 add-flow s1 "priority=100,in_port=1,actions=drop"
```

**Meaning of the rule:**

  Field            Meaning
  ---------------- ------------------------------------------------
  `priority=100`   Priority assigned to this rule
  `in_port=1`      Matches packets entering through switch port 1
  `actions=drop`   Drops matching packets

### Step 12.3 --- Test the filtering behaviour

Return to the Mininet terminal:

``` text
h1 ping h2
```

If H1 is connected to port 1, its outgoing packets should be dropped by
the rule, and the ping should fail.

You can inspect the flow table again from Terminal 3:

``` bash
sudo ovs-ofctl -O OpenFlow13 dump-flows s1
```

------------------------------------------------------------------------

## 13. Remove Rules and Clean Up

### Step 13.1 --- Delete flow entries (Terminal 3)

``` bash
sudo ovs-ofctl -O OpenFlow13 del-flows s1
```

This deletes the switch's flow entries, including entries installed by
the controller.

> **Important:** Deleting all flows also removes the controller's
> table-miss entry. If you want to repeat the normal connectivity test,
> exit Mininet, clean up, and start the topology again while the Ryu
> controller is running.

### Step 13.2 --- Exit Mininet

At the `mininet>` prompt:

``` text
exit
```

### Step 13.3 --- Clean up Mininet (Ubuntu terminal)

``` bash
sudo mn -c
```

This removes Mininet-created interfaces and cleans up its processes.

The experiment is now complete. You may stop the Ryu controller with
**Ctrl + C** in Terminal 1.

------------------------------------------------------------------------

## 14. Troubleshooting

  --------------------------------------------------------------------------------
  Problem / message                   What to check
  ----------------------------------- --------------------------------------------
  `pip install ryu` fails             Do not use the unqualified system `pip`
                                      route. Follow the Python 3.9 and Faucet Ryu
                                      installation steps in this guide.

  `get_script_args` or setuptools     Confirm that `setuptools==57.5.0` was
  build error                         installed in `~/ryu-env39` and that the Ryu
                                      installation used `--no-build-isolation`.

  `ryu-manager: command not found`    Activate `source ~/ryu-env39/bin/activate`,
                                      or run `~/ryu-env39/bin/ryu-manager`
                                      directly.

  Python reports version 3.14         The Ryu environment is not active. Check
                                      `python --version` after activation.

  `ryu.app.simple_switch_13` cannot   Recheck the Ryu installation and runtime
  be imported                         dependencies using the verification
                                      commands.

  Mininet cannot contact the          Ensure Terminal 1 is still running and that
  controller                          Mininet uses controller address `127.0.0.1`
                                      and port `6653`.

  Open vSwitch connection/database    Check
  error                               `sudo systemctl status openvswitch-switch`
                                      and `sudo ovs-vsctl show`.

  Mininet reports stale interfaces or Exit Mininet and run `sudo mn -c`.
  processes                           

  Filtering rule does not block H1    Run `net` and confirm H1's actual switch
                                      port. The example rule assumes port 1.
  --------------------------------------------------------------------------------

### Useful diagnostic commands

``` bash
# Check Python used by the active environment
python --version

# Check Ryu version
~/ryu-env39/bin/ryu-manager --version

# Check Open vSwitch
sudo ovs-vsctl show

# Check Open vSwitch service
sudo systemctl status openvswitch-switch

# Check flow entries
sudo ovs-ofctl -O OpenFlow13 dump-flows s1

# Clean Mininet
sudo mn -c
```

------------------------------------------------------------------------

## 15. Quick Command Reference

### Step 15.1 --- System Preparation & Tooling (Terminal 1)

Execute each command one by one:

``` bash
sudo apt update
```

``` bash
sudo apt upgrade -y
```

``` bash
sudo apt install -y git curl ca-certificates build-essential mininet openvswitch-switch python3 python3-pip python3-venv
```

``` bash
sudo systemctl enable --now openvswitch-switch
```

``` bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

``` bash
source "$HOME/.local/bin/env"
```

``` bash
uv python install 3.9
```

``` bash
uv venv --python 3.9 ~/ryu-env39
```

``` bash
source ~/ryu-env39/bin/activate
```

``` bash
uv pip install --python ~/ryu-env39/bin/python setuptools==57.5.0 wheel
```

``` bash
uv pip install --python ~/ryu-env39/bin/python --no-build-isolation --no-deps "git+https://github.com/faucetsdn/ryu.git@v4.34"
```

``` bash
uv pip install --python ~/ryu-env39/bin/python eventlet msgpack netaddr oslo.config ovs routes six tinyrpc webob
```

``` bash
~/ryu-env39/bin/ryu-manager --version
```

``` bash
~/ryu-env39/bin/python -c "import ryu.app.simple_switch_13; print('OpenFlow 1.3 application OK')"
```

---

### Step 15.2 --- Experiment Execution (Across Terminals)

#### Terminal 1: Launch Ryu Controller

``` bash
source ~/ryu-env39/bin/activate
```

``` bash
ryu-manager --ofp-tcp-listen-port 6653 ryu.app.simple_switch_13
```

*(Keep Terminal 1 running)*

#### Terminal 2: Launch Mininet CLI

``` bash
sudo mn --topo single,3 --controller=remote,ip=127.0.0.1,port=6653 --switch ovsk,protocols=OpenFlow13
```

Inside the `mininet>` prompt, execute one by one:

``` text
nodes
```

``` text
net
```

``` text
pingall
```

``` text
h1 ping h2
```

*(Press `Ctrl + C` in Terminal 2 after verifying ping replies)*

#### Terminal 3: Inspect OpenFlow & Flow Rule Injection

Open Terminal 3:

``` bash
sudo ovs-ofctl -O OpenFlow13 dump-flows s1
```

``` bash
sudo ovs-ofctl -O OpenFlow13 add-flow s1 "priority=100,in_port=1,actions=drop"
```

Back in **Terminal 2 (Mininet CLI)**, test that packets from `h1` are dropped:

``` text
h1 ping h2
```

*(Notice 100% packet drop; press `Ctrl + C`)*

In **Terminal 3**, delete the rules:

``` bash
sudo ovs-ofctl -O OpenFlow13 del-flows s1
```

In **Terminal 2 (Mininet CLI)**, exit:

``` text
exit
```

In **Terminal 2 (Ubuntu Shell)**, clean up Mininet:

``` bash
sudo mn -c
```

Now you can stop the Ryu controller in **Terminal 1** by pressing `Ctrl + C`.

------------------------------------------------------------------------

## 16. Expected Result

The SDN environment is configured using Mininet, Open vSwitch, and the
Ryu SDN Controller. A single-switch topology containing three virtual
hosts is created. Network connectivity is tested using `pingall` and
individual host ping commands. OpenFlow 1.3 flow entries are inspected,
and a traffic-filtering rule is added to demonstrate packet dropping.

**Result:** The SDN environment is successfully created, and basic
forwarding and traffic-filtering operations are demonstrated.
