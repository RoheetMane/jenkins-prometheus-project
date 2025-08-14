# Jenkins + Prometheus + Grafana Setup Guide

## 1. Get API Key

1. Log in to your profile and select **Settings**.
2. Click **API** from the left-side panel.
3. Create a new API key by clicking **Create** and accepting the terms and conditions.
4. Provide the required basic details and click **Submit**.

**TMDB API-KEY:**



---

## 2. Launch EC2 (Ubuntu 22.04)

- Provision an EC2 instance on AWS with **Ubuntu 22.04**.
- Connect to the instance using SSH.

---

## 3. Install Jenkins for Automation

```bash
sudo apt update
sudo apt install fontconfig openjdk-21-jre -y

sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install jenkins
sudo systemctl start jenkins
sudo systemctl enable jenkins



## 4. Install Docker and Run the App in a Container

sudo apt-get update
sudo apt-get install docker.io -y
sudo systemctl start docker
sudo usermod -aG docker ubuntu
sudo usermod -aG docker jenkins
newgrp docker
sudo chmod 777 /var/run/docker.sock
