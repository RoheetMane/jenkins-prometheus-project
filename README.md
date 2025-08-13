# jenkins-prometheus-project
# 🚀 Jenkins + Prometheus CI/CD Project

## 📌 Overview
This project demonstrates a complete **CI/CD pipeline** setup with **Jenkins**, **Prometheus**, and **Grafana** for monitoring.  
It includes:
- CI/CD automation with Jenkins pipelines
- Static code analysis with SonarQube
- Vulnerability scanning with Trivy & OWASP Dependency Check
- Docker-based application deployment
- Monitoring & alerting with Prometheus & Grafana

---

## 🖥 Project Architecture
![Architecture Diagram](https://raw.githubusercontent.com/RoheetMane/jenkins-prometheus-project/images/architecture.png)

---

## ⚙️ Prerequisites
Before starting, ensure you have:
- AWS Account (EC2, Security Groups, IAM Roles)
- DockerHub account for storing images
- GitHub repository for source code
- Basic knowledge of Linux & Docker commands

---

## 🛠 Step-by-Step Implementation

### **1. Launch EC2 Instance (Ubuntu 22.04)**
```bash
sudo apt update
sudo apt upgrade -y


---

### 2️⃣ Launch EC2 (Ubuntu 22.04)
Provision an EC2 instance on AWS with Ubuntu 22.04 and connect via SSH:
```bash
ssh -i mykey.pem ubuntu@<public-ip>

