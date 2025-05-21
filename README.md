🎓 AWS DevOps Capstone Project - Blog Application Deployment on AWS
This project is a comprehensive DevOps Capstone developed as the final deliverable for the SDA Infrastructure Engineering Bootcamp. It demonstrates the complete deployment of a Django-based Blog Web Application on Amazon Web Services (AWS) using industry-standard DevOps practices and tools including Terraform, Ansible, Prometheus, and Grafana.

It simulates a production-grade infrastructure with automation, scalability, monitoring, and cloud-native configurations – showcasing real-world DevOps engineering skills.

🎯 Project Objective
The main objective of this capstone project is to build and deploy a highly available, observable, and secure blog application on AWS using Infrastructure as Code (IaC) principles.

🔧 Key Features:
High Availability: Achieved via Application Load Balancer (ALB) and Auto Scaling Groups (ASG)

Secure Backend: Application backend runs on EC2 instances within private subnets

Media Storage: User-uploaded images and videos are stored in an Amazon S3 bucket

Database Layer: Managed using Amazon RDS (MySQL) within private subnets

Infrastructure Provisioning: All AWS resources are created using Terraform

Configuration Management: Automated setup and configurations using Ansible

Monitoring & Observability: Integrated Prometheus and Grafana to track system performance

Secrets Management: Handled securely using AWS SSM Parameter Store

🌐 Live Demo
You can access the deployed blog application via the Application Load Balancer from the link below:
Click to view the live demo

## 🧱 Architecture Overview

![Architecture Diagram](screenshots/architecture.png)

- ✅ Custom VPC with public/private subnets across 2 Availability Zones  
- ✅ EC2 instances managed via Launch Template + Auto Scaling Group  
- ✅ Application Load Balancer for routing  
- ✅ Amazon RDS (MySQL) in private subnets with subnet group  
- ✅ S3 for media storage  
- ✅ Prometheus & Grafana for monitoring  
- ✅ Terraform for infrastructure as code  
- ✅ Ansible for server configuration

---

## 🛠️ Tools & Services

| Tool         | Purpose                                      |
|--------------|----------------------------------------------|
| AWS VPC      | Isolated network for app infrastructure      |
| EC2 + ASG    | Hosting Django app with auto scaling         |
| ALB          | Routing HTTP traffic to backend instances    |
| RDS (MySQL)  | Storing blog data securely                   |
| S3           | Media (images, videos) storage               |
| Terraform    | Infrastructure provisioning                  |
| Ansible      | App config + Node Exporter setup             |
| Prometheus   | Collecting EC2 metrics                       |
| Grafana      | Visualizing system metrics                   |
| GitHub + SSM | Hosting source + managing secrets            |

---

## 🧩 Step-by-Step Implementation

### 1️⃣ VPC and Network Setup

- Custom VPC CIDR: `10.90.0.0/16`
- Public subnets:
  - `10.90.10.0/24` (AZ1)
  - `10.90.20.0/24` (AZ2)
- Private subnets:
  - `10.90.11.0/24` (AZ1)
  - `10.90.21.0/24` (AZ2)
- Internet Gateway for public subnets
- NAT Gateway for private subnets
- Public route table + Private route table

---

### 2️⃣ Security Groups

- **ALB SG**: HTTP/HTTPS from `0.0.0.0/0`
- **EC2 SG**: HTTP from ALB SG, SSH from admin IP
- **RDS SG**: MySQL access from EC2 SG only

---

### 3️⃣ SSM Parameter Store

Parameters stored securely:
- `/nawal/capstone/username`
- `/nawal/capstone/password`
- `/nawal/capstone/token`

---

### 4️⃣ RDS Setup

- Engine: MySQL  
- DB Name: `clarusway`  
- Subnet Group: `Private 1a + 1b`  
- No public access  
- Passwords fetched via SSM

---

### 5️⃣ S3 Media Storage

- Private bucket created
- Accessed only from the Django app
- Bucket policy adjusted for limited read access

---

### 6️⃣ GitHub Integration

- Repo: `infrastructure-capstone`
- Used GitHub Personal Access Token stored in SSM
- Git operations automated in `userdata.sh`

---

### 7️⃣ User Data Script

`userdata.sh` tasks:
- Install dependencies (`git`, `python3`, `pip`)
- Fetch project repo using token
- Install Django dependencies
- Apply migrations + start server with `gunicorn`

---

### 8️⃣ Manual Testing EC2

- Launched test instance in public subnet
- Connected to RDS successfully
- Verified uploads to S3 worked

---

### 9️⃣ Terraform Deployment


terraform init
terraform apply
Creates:

VPC, Subnets, IGW/NAT

EC2 Launch Template + ASG

ALB + Listener

RDS + Subnet Group

IAM Roles + Policies

Output DNS for blog app

📸 Screenshot:


🔟 SDA-Admin-Node (Monitoring)
EC2 instance in public subnet

Tools installed: Terraform, Ansible, Prometheus, Grafana

Dynamic Ansible inventory used

📸 Screenshot (Prometheus UI):


📸 Screenshot (Grafana):


1️⃣1️⃣ Prometheus + Grafana
EC2 discovery using ec2_sd_configs

Dashboard ID: 1860 imported

Metrics for:

CPU, Memory, Disk, Uptime

Network activity

📸 Node Exporter Access:


📸 Metrics Query:


💡 Key Features
✔️ High Availability with Multi-AZ
✔️ Scalable backend with Auto Scaling
✔️ RDS MySQL in private subnet
✔️ S3 media uploads via secure access
✔️ Infrastructure as Code with Terraform
✔️ Automated provisioning with Ansible
✔️ Monitoring using Prometheus & Grafana

⚠️ Challenges & Fixes
Challenge	Fix
EC2 Health Check Failure	Fixed userdata.sh & SG rules
Node Exporter Down	Restarted using Ansible handler
S3 Media Not Displayed	Corrected bucket policy + CORS
DB Auth Fails	Used SSM Parameters in Django config

📁 Project Structure
bash
Copy code
.
├── terraform/                  # Infrastructure as Code
├── ansible/                    # Configuration scripts
├── scripts/                    # User data and helpers
├── screenshots/                # Evidence for demo
└── README.md
🧠 Lessons Learned
💡 Secure secrets via SSM Parameter Store

💡 Debugging ALB is easier via health check logs

💡 Node Exporter must run as a service

💡 Don't hardcode anything — automate everything!

📬 How to Run
Clone the repo

Configure AWS access (aws configure)

Upload your SSH KeyPair to AWS

Run:

bash
Copy code
cd terraform/
terraform init
terraform apply
Then configure app with Ansible:

bash
Copy code
ansible-playbook -i inventory_aws_ec2.yml prometheus_monitoring_setup.yaml
🧹 Cleanup
To destroy infrastructure:

bash
Copy code
terraform destroy --auto-approve
Also remove:

RDS instance (from console)

Empty & delete S3 bucket

Terminate NAT Gateway (to avoid cost)

Delete IAM Roles manually

Delete all SSM Parameters

👥 Team
Team SDA – 12 Members
Each member contributed to one or more components: provisioning, automation, backend, monitoring, or documentation.

🙏 Final Words
This project reflects a production-ready DevOps deployment on AWS using modern tools and best practices.

Thanks for reading 💙
