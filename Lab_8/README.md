<!-- markdownlint-disable -->

<div align="center">

# ☁️ Experiment 8: Implementing Applications using Google App Engine (PaaS)

![Header Wave](https://capsule-render.vercel.app/api?type=waving&color=0:090d16,35:0284c7,70:6366f1,100:090d16&height=180&section=header&text=EXPERIMENT%208&fontSize=36&fontColor=ffffff&fontAlignY=38&desc=Python%20Flask%20App%20Deployment,%20Versioning%20&%20Monitoring%20on%20GCP&descAlignY=60)

[![Course](https://img.shields.io/badge/Course-CSE12045-0284c7?style=flat-square&logo=googlecloud&logoColor=white)](https://adamasuniversity.ac.in/)
[![Platform](https://img.shields.io/badge/PaaS-Google%20App%20Engine-4285F4?style=flat-square&logo=googleappengine&logoColor=white)](https://cloud.google.com/appengine)
[![Runtime](https://img.shields.io/badge/Runtime-Python%203.12-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![Framework](https://img.shields.io/badge/Framework-Flask%203.1.3-000000?style=flat-square&logo=flask&logoColor=white)](https://flask.palletsprojects.com/)
[![WSGI Server](https://img.shields.io/badge/Server-Gunicorn%2027.0.1-499848?style=flat-square&logo=gunicorn&logoColor=white)](https://gunicorn.org/)
[![Cloud Shell](https://img.shields.io/badge/CLI-Google%20Cloud%20SDK-FBBC04?style=flat-square&logo=googlecloud&logoColor=black)](https://cloud.google.com/sdk)
[![PDF Report](https://img.shields.io/badge/Report-Experiment__8.pdf-red?style=flat-square&logo=adobeacrobatreader&logoColor=white)](Experiment_8.pdf)
[![Back to Root](https://img.shields.io/badge/Back%20to-Main%20README-10b981?style=flat-square&logo=github&logoColor=white)](../README.md)

</div>

---

## 🎯 1. Objective

To explore, implement, and evaluate the **Platform as a Service (PaaS)** cloud computing paradigm using **Google App Engine (GAE)** on **Google Cloud Platform (GCP)**. The laboratory demonstrates the complete lifecycle of cloud-native development: creating a Python Flask web application, pre-deployment validation in Google Cloud Shell on port 8080, packaging declarative `app.yaml` runtime descriptors, deploying to App Engine Standard Environment (`python312`), managing progressive releases and multi-version traffic allocation, tailing live streaming telemetry logs, and monitoring real-time load, request metrics, and automatic horizontal scaling.

---

## 🏗️ 2. Architectural Hierarchy

Google App Engine abstracts away virtual machine management, hypervisor patching, and load balancer provisioning through a four-tier hierarchical operational model:

```mermaid
graph TD
    App["Application: gen-lang-client-0745553710 (us-central)"]
    Svc["Service: default (Web Application Tier)"]
    V1["Version 1: 20261118t201936 (0% Traffic)"]
    V2["Version 2: 20261118t202419 (100% Traffic)"]
    Inst["Dynamic Autoscaled Instance (145 MB Memory Footprint)"]

    App --> Svc
    Svc --> V1
    Svc --> V2
    V2 --> Inst
```

---

## 🛠️ 3. Step-by-Step Implementation

### Step 3.1 — Cloud Shell Workspace Setup & File Creation
Open Google Cloud Shell and create an isolated working directory `cloud-lab8`:
```bash
mkdir cloud-lab8 && cd cloud-lab8
```

1. **Create the Flask Controller (`main.py`)**:
   ```python
   from flask import Flask

   app = Flask(__name__)

   @app.route("/")
   def home():
       return """
       <h1>Cloud Computing Lab 8</h1>
       <p>Application successfully implemented using Google App Engine.</p>
       <p>This application demonstrates Platform as a Service (PaaS).</p>
       """

   if __name__ == "__main__":
       app.run(host="127.0.0.1", port=8080)
   ```

2. **Create the Dependency Manifest (`requirements.txt`)**:
   ```text
   Flask
   gunicorn
   ```

3. **Create the Deployment Descriptor (`app.yaml`)**:
   ```yaml
   runtime: python312
   ```

### Step 3.2 — Local Verification via Web Preview
Install the local dependencies and run the development server:
```bash
pip install Flask && python main.py
```
Open **Web Preview** on port `8080` in Cloud Shell (`https://8080-cs-905738656321-default.cs-asia-southeast1-fork.cloudshell.dev`) to verify that the local HTTP endpoints respond with HTTP 200.

### Step 3.3 — Live App Engine Deployment (Version 1)
Deploy the application to the App Engine Standard Environment:
```bash
gcloud app deploy
```
- **Target Project:** `gen-lang-client-0745553710`
- **Target Service:** `default`
- **Target Version:** `20261118t201936`
- **Production URL:** [https://gen-lang-client-0745553710.uc.r.appspot.com](https://gen-lang-client-0745553710.uc.r.appspot.com)

### Step 3.4 — Real-Time Log Streaming & Observability
Stream live application logs, Gunicorn WSGI startup events, and incoming HTTP requests:
```bash
gcloud app logs tail -s default
```
```text
[INFO] Starting gunicorn 27.0.1
[INFO] Listening at: http://0.0.0.0:8081 (14)
[INFO] Using worker: sync
[INFO] Booting worker with pid: 15
GET 200 137 B https://gen-lang-client-0745553710.uc.r.appspot.com/
```

### Step 3.5 — Progressive Delivery & Multi-Version Rollout (Version 2)
Update `main.py` with updated styling for Version 2:
```python
cat << 'EOF' > main.py
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return """
    <h2>Welcome to Cloud Computing Lab 8</h2>
    <h3>Application Version 2</h3>
    <p>Successfully deployed using Google App Engine.</p>
    """

if __name__ == "__main__":
    app.run(host="127.0.0.1", port=8080)
EOF
```
Redeploy the application:
```bash
gcloud app deploy
```
App Engine deploys version `20261118t202419` and shifts 100% of live production traffic with zero downtime, maintaining version `20261118t201936` for instant rollback.

---

## 📊 4. Cloud Monitoring & Telemetry

| Metric / Parameter | Value Observed | Cloud Console Location |
| :--- | :--- | :--- |
| **Project Workspace** | `gen-lang-client-0745553710` | App Engine Header |
| **Deployment Region** | `us-central` | App Engine Dashboard |
| **Traffic Throughput** | `0.0833 requests / second` | Real-time traffic graph |
| **Total Requests (24h)** | `7 requests` | Current Load Table |
| **Average Latency** | `551 ms` | Current Load Table |
| **Active Service** | `default` (Ingress: All) | Services Overview |
| **Active Versions** | `2` (`20261118t202419` 100%, `20261118t201936` 0%) | Versions Management |
| **Dynamic Instances** | `1` autoscaled (`145 MB` memory footprint) | Instances Dashboard |

---

## 📑 5. Lab Report & Documentation

The comprehensive formal laboratory report has been compiled and styled in accordance with academic standards:
- **Formal PDF Report:** [📄 Experiment_8.pdf](Experiment_8.pdf) *(12 pages, Comic Sans MS typography, 12 embedded visual figures, command tables, and service comparison matrices)*

---

<div align="center">

[⬅️ Back to Main Repository](../README.md)

</div>
