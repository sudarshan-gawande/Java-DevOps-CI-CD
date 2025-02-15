# Jenkins Setup Guide for AWS with Slave Architecture 🚀

## 📋 Overview
This document provides a step-by-step guide to setting up **Jenkins** on AWS with a **master-slave architecture** for efficient CI/CD automation. It includes required software installations, configurations, and best practices for a production-ready Jenkins environment.

---

## 🏷️ Architecture Diagram

![Jenkins AWS Slave Architecture](../images/jenkins_architecture.png)

---

## 📌 Prerequisites

### ✅ AWS Setup
- **Jenkins Master**: An AWS EC2 instance (Ubuntu 22.04 recommended) to run Jenkins.
- **Jenkins Slaves**: Separate AWS EC2 instances (Ubuntu 22.04 recommended) to execute builds.
- **Security Group Configuration**:
  - Allow **Port 8080** for Jenkins UI on the Master.
  - Allow **Port 22** for SSH access (for Jenkins Slave Nodes and remote administration).
  - Allow **Port 50000** for Jenkins Agent Communication between Master and Slaves.

### ✅ Required Software
- **Jenkins (LTS Version) installed on Master**
- **Java 17 (Amazon Corretto recommended) installed on both Master and Slaves**
- **Maven & Gradle** (for Java builds, installed on both Master and Slaves)
- **Git** (for source code versioning, installed on both Master and Slaves)
- **Docker** (optional, for containerized builds)
- **SonarQube Scanner** (for code analysis)
- **Nexus CLI** (for artifact management)

---

## 🔧 Jenkins Master Setup (AWS EC2 Instance)

### **1⃣ Install Java 17**
```bash
sudo apt update
sudo apt install -y openjdk-17-jdk
java -version
```

### **2⃣ Install Jenkins (Updated for Ubuntu 22.04)**
```bash
# Add the Jenkins repository key using the new method
sudo mkdir -p /usr/share/keyrings
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null

# Add the Jenkins repository to sources.list.d
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] http://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

# Update the package list
sudo apt update

# Install Jenkins
sudo apt install -y jenkins

# Start Jenkins service
sudo systemctl start jenkins
sudo systemctl enable jenkins

# Check Jenkins status
sudo systemctl status jenkins
```

### **3⃣ Setup Firewall Rules**
```bash
sudo ufw allow 8080
sudo ufw allow OpenSSH
sudo ufw enable
```

### **4⃣ Get Jenkins Admin Password**
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

- Access **Jenkins UI**: `http://<EC2-PUBLIC-IP>:8080`
- Enter the **Admin Password**.
- Install recommended plugins.

---

## 🔧 Jenkins Slave Node Setup (AWS EC2 Instance) (Running on a Different Machine)

### **1. Install Java & Required Tools**
```bash
sudo apt update
sudo apt install -y openjdk-17-jdk git maven gradle
java -version
```
### **2. Configure Jenkins Master to Recognize Slave**
- In **Jenkins UI**:
  - Go to **Manage Jenkins** → **Manage Nodes and Clouds**.
  - Click **New Node** → Enter Slave Name.
  - Choose **Permanent Agent**.
  - Set Remote Root Directory (`/home/ubuntu/slavenode1`).
  - Choose **Launch method: SSH**.
  - Enter **Slave's Public IP & SSH credentials**.
  - Select **Manually Trusted Key Verification Strategy**.
  - Set Availability to **Keep this agent online as much as possible**.
  - Save & Test Connection.
---

## 🔥 Installing Required Plugins

### **Recommended Jenkins Plugins**
```bash
sudo su - jenkins
wget http://updates.jenkins-ci.org/download/plugins/git/latest/git.hpi -P ~/.jenkins/plugins/
wget http://updates.jenkins-ci.org/download/plugins/maven-plugin/latest/maven-plugin.hpi -P ~/.jenkins/plugins/
```
Or install via Jenkins UI:
- **Git Plugin**
- **Pipeline Plugin**
- **SonarQube Scanner Plugin**
- **Nexus Artifact Uploader**
- **Docker Plugin** (if using Docker)
- **SSH Build Agents** (for slave nodes)

---

## 🏁 Final Verification

### ✅ **Jenkins Master (Running on a Separate Machine)**
- Jenkins UI is accessible at `http://<EC2-PUBLIC-IP>:8080`
- Slave nodes are successfully connected.
- Required plugins are installed.

### ✅ **Jenkins Slave Nodes (Running on Separate EC2 Instances)**
- Slave agents are running.
- Slave can execute build jobs.

![Jenkins Dashboard](../images/jenkins_dashboard.png)

---

## 📄 Additional Documentation
- **[Jenkins Official Docs](https://www.jenkins.io/doc/)**
- **[AWS EC2 Setup Guide](https://docs.aws.amazon.com/ec2/)**
- **[Jenkins with AWS CodeDeploy](https://docs.aws.amazon.com/codedeploy/latest/userguide/integrating-jenkins-with-codedeploy.html)**

---

## 📧 Contact
📧 **Email**: [sudarshangawande98@gmail.com](mailto:sudarshangawande98@gmail.com)  
📎 **GitHub**: [Sudarshan Gawande](https://github.com/sudarshangawande98)

---

## 📄 License
This project is licensed under the **MIT License**. See the `LICENSE` file for details.
