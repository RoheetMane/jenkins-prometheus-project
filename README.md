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

sudo apt update
sudo apt install fontconfig openjdk-21-jre -y

wget -O /usr/share/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins


4️⃣ Install Docker & Run Application

sudo apt-get update
sudo apt-get install docker.io -y
sudo systemctl start docker
sudo usermod -aG docker ubuntu
sudo usermod -aG docker jenkins
newgrp docker
sudo chmod 777 /var/run/docker.sock

🔐 Phase 2: Security
Install SonarQube

docker run -d --name sonar -p 9000:9000 sonarqube:lts-community

Access at:

http://<public-ip>:9000
Username: admin
Password: admin

Install Trivy
File scan:

trivy fs . > trivyfs.txt

Image scan:

trivy image roheetmane7/netflix:latest > trivyimage.txt

⚙️ Jenkins Pipeline Example

pipeline {
    agent any
    tools {
        jdk 'jdk17'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        TMDB_API_KEY = credentials('tmdb-api-key')
    }
    stages {
        stage('Clean Workspace') {
            steps { cleanWs() }
        }
        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/abhipraydhoble/netflix.git'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.projectName=Netflix \
                        -Dsonar.projectKey=Netflix'''
                }
            }
        }
        stage('Quality Gate') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }
        stage('Install Dependencies') {
            steps { sh 'npm install' }
        }
        stage('TRIVY FS Scan') {
            steps { sh 'trivy fs . > trivyfs.txt' }
        }
        stage('Docker Build & Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build --build-arg TMDB_V3_API_KEY=${TMDB_API_KEY} -t netflix ."
                        sh "docker tag netflix roheetmane7/netflix:latest"
                        sh "docker push roheetmane7/netflix:latest"
                    }
                }
            }
        }
        stage('TRIVY Image Scan') {
            steps { sh 'trivy image roheetmane7/netflix:latest > trivyimage.txt' }
        }
        stage('Deploy to Container') {
            steps { sh 'docker run -d --name netflix -p 8081:80 roheetmane7/netflix:latest' }
        }
    }
}





📊 Phase 4: Monitoring
Install Prometheus

sudo useradd --system --no-create-home --shell /bin/false prometheus
wget https://github.com/prometheus/prometheus/releases/download/v2.47.1/prometheus-2.47.1.linux-amd64.tar.gz
tar -xvf prometheus-2.47.1.linux-amd64.tar.gz
sudo mkdir -p /data /etc/prometheus
sudo mv prometheus promtool /usr/local/bin/
sudo mv consoles/ console_libraries/ /etc/prometheus/
sudo mv prometheus.yml /etc/prometheus/prometheus.yml
sudo chown -R prometheus:prometheus /etc/prometheus/ /data/


Install Grafana

sudo apt-get update
sudo apt-get install -y apt-transport-https software-properties-common
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
sudo apt-get update
sudo apt-get -y install grafana
sudo systemctl enable grafana-server
sudo systemctl start grafana-server



