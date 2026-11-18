<!-- markdownlint-disable -->

<div align="center">

# ☁️ Experiment 6: Familiarization and Usage of Cloud Services Using AWS

![Header Wave](https://capsule-render.vercel.app/api?type=waving&color=0:090d16,35:0284c7,70:6366f1,100:090d16&height=180&section=header&text=EXPERIMENT%206&fontSize=36&fontColor=ffffff&fontAlignY=38&desc=Familiarization%20and%20Usage%20of%20Cloud%20Services%20Using%20AWS&descAlignY=60)

[![Course](https://img.shields.io/badge/Course-CSE12045-0284c7?style=flat-square&logo=googlecloud&logoColor=white)](https://adamasuniversity.ac.in/)
[![Provider](https://img.shields.io/badge/Cloud-Amazon%20Web%20Services-FF9900?style=flat-square&logo=amazonwebservices&logoColor=white)](https://aws.amazon.com/)
[![CLI](https://img.shields.io/badge/Tool-AWS%20CLI%20v2-232F3E?style=flat-square&logo=amazonwebservices&logoColor=white)](https://aws.amazon.com/cli/)
[![Region](https://img.shields.io/badge/Region-ap--southeast--2-blue?style=flat-square)](https://aws.amazon.com/)
[![PDF Report](https://img.shields.io/badge/Report-Experiment__6.pdf-red?style=flat-square&logo=adobeacrobatreader&logoColor=white)](Experiment_6.pdf)
[![Back to Root](https://img.shields.io/badge/Back%20to-Main%20README-10b981?style=flat-square&logo=github&logoColor=white)](../README.md)

</div>

---

## 🎯 1. Objective

To gain hands-on operational experience with enterprise public cloud services using **Amazon Web Services (AWS)**, **Ubuntu Linux**, and the **AWS CLI v2**. The experiment covers object storage provisioning (**Amazon S3**), identity and role-based access governance (**AWS IAM**), compute virtualization (**Amazon EC2**), software-defined network isolation (**Amazon VPC**), operational observability (**Amazon CloudWatch**), and programmatic cloud administration.

---

## 🌐 2. Evaluated AWS Core Cloud Services

| AWS Service | Category | Architectural Role Evaluated |
| :--- | :--- | :--- |
| **Amazon S3** | Object Storage | Created private bucket `cse12045-cloud-lab6-babin114`, uploaded/downloaded objects, verified encryption & Block Public Access |
| **AWS IAM** | Security & Identity | Evaluated 11 service roles, 1,600+ policies, JSON access statements, and Least Privilege principles |
| **Amazon EC2** | Compute (IaaS) | Inspected regional compute instances, AMIs (Ubuntu/Amazon Linux), instance types (`t2.micro`), and security groups |
| **Amazon VPC** | Networking | Analyzed default VPC, multi-AZ subnets (ap-southeast-2a/b/c), route tables, and Internet Gateways |
| **Amazon CloudWatch** | Monitoring | Evaluated Classic metrics across HealthLake, Logs, and Usage (159 metrics), plus automated alarm thresholding |
| **AWS CLI v2** | Developer CLI | Configured credentials, verified identity via STS, automated S3 data lifecycle (`cp`, `ls`, `rm`, `rb`) |

---

## 🛠️ 3. Step-by-Step Procedure & CLI Operations

### Step 3.1 — Install & Configure AWS CLI in Ubuntu
```bash
# Update Ubuntu environment
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl unzip git build-essential

# Download and install AWS CLI v2
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version

# Configure AWS Credentials
aws configure
aws sts get-caller-identity
```

### Step 3.2 — Amazon S3 Object Storage Lifecycle
```bash
# List accessible buckets
aws s3 ls

# Create local test file
mkdir -p ~/cloud-lab6-cli && cd ~/cloud-lab6-cli
echo "AWS CLI Cloud Storage Test" > sample.txt

# Upload object to S3 bucket
aws s3 cp sample.txt s3://cse12045-cloud-lab6-babin114/sample.txt

# List remote bucket contents
aws s3 ls s3://cse12045-cloud-lab6-babin114/

# Download object and verify integrity
aws s3 cp s3://cse12045-cloud-lab6-babin114/sample.txt downloaded.txt
cat downloaded.txt

# Clean up resources (Delete objects and remove bucket)
aws s3 rm s3://cse12045-cloud-lab6-babin114/sample.txt
aws s3 rm s3://cse12045-cloud-lab6-babin114/lab-note.txt
aws s3 rb s3://cse12045-cloud-lab6-babin114
aws s3 ls
```

---

## 📊 4. Result & Cost Discipline

- **End-to-End Verification:** Configured storage, security, compute, networking, and monitoring components on AWS.
- **Data Integrity:** Verified 100% bit-level consistency across GUI and CLI object uploads and downloads.
- **Resource Hygiene:** Completely flushed S3 test objects and removed the bucket, maintaining zero residual cloud costs.
- **Full Report:** 25 detailed screenshots covering both the AWS Console and Ubuntu CLI are documented in [Experiment_6.pdf](Experiment_6.pdf).

---

<div align="center">

[⬅️ Back to Main Repository](../README.md)

</div>
