# Jenkins Master-Slave Setup Guide 🚀

## 📋 Overview
This document provides a step-by-step guide to setting up **Jenkins** with a **master-slave architecture** for efficient CI/CD automation. It includes required software installations, configurations, and best practices for a production-ready Jenkins environment.

---

## 🏛️ Architecture Diagram

![Jenkins Master-Slave Architecture](../../images/jenkins_architecture.png)

---

## 📌 Prerequisites

### ✅ System Requirements
- **Jenkins Master**: An Ubuntu 22.04 machine to run Jenkins.
- **Jenkins Slaves**: Separate Ubuntu 22.04 machines to execute builds.
- **Instance Type**: Use **t3.medium** for Jenkins Slave nodes (t3.micro is not sufficient).
- **Security Group Configuration**:
  - Allow **Port 8080** for Jenkins UI on the Master.
  - Allow **Port 22** for SSH access (for Jenkins Slave Nodes and remote administration).
  - Allow **Port 50000** for Jenkins Agent Communication between Master and Slaves.

### ✅ Required Software
- **Jenkins (LTS Version) installed on Master**
- **Java 8 & Java 17 installed on both Master and Slaves**
- **Maven & Gradle** (for Java builds, installed on both Master and Slaves)
- **Git** (for source code versioning, installed on both Master and Slaves)
- **Docker** (optional, for containerized builds)
- **SonarQube Scanner** (for code analysis)
- **Nexus CLI** (for artifact management)

---

## 🔧 Jenkins Master Setup

### **1⃣ Install Java 17**
```bash
sudo apt update
sudo apt install -y openjdk-17-jdk
java -version
```

### **2⃣ Install Jenkins (for Ubuntu 22.04)**
```bash
# Add the Jenkins repository key using
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

### **3⃣ Setup Firewall Rules (If Required)**
```bash
sudo ufw allow 8080
sudo ufw allow OpenSSH
sudo ufw enable
```

### **4⃣ Get Jenkins Admin Password**
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

- Access **Jenkins UI**: `http://<JENKINS_MASTER_IP>:8080`
- Enter the **Admin Password**.
- Install recommended plugins.

---

## 🔧 Jenkins Slave Node Setup

### **1. Install Java 8 & Java 17**
```bash
sudo apt update
sudo apt install -y openjdk-8-jdk openjdk-17-jdk
java -version
```

### **2. Configure Java Alternatives**
To manage multiple Java versions, use `update-alternatives`:
```bash
sudo update-alternatives --config java
```
Select the appropriate version and verify:
```bash
java -version
```

### **3. Configure SSH Credentials for Slave Node**
- In **Jenkins UI**:
  - Go to **Manage Jenkins** → **Manage Credentials**.
  - Select **Global credentials (unrestricted)**.
  - Click **Add Credentials**.
  - Choose **SSH Username with Private Key**.
  - Enter **ID** (e.g., `Slave1Credential`).
  - Enter **Description** (e.g., `Slave1Credential`).
  - Enter **Username** (e.g., `ubuntu`).
  - Select **Enter Directly** for Private Key.
  - Paste the contents of the .pem file (e.g., jenkins-slave.pem).
  - Click **Add** to save credentials.

![Jenkins Slave Credentials Setup](../../images/jenkins_slave_credentials.png)

### **4. Configure Jenkins Master to Recognize Slave**
- In **Jenkins UI**:
  - Go to **Manage Jenkins** → **Manage Nodes and Clouds**.
  - Click **New Node** → Enter Slave Name.
  - Choose **Permanent Agent**.
  - Set Remote Root Directory (`/home/ubuntu/slavenode1`).
  - Choose **Launch method: SSH**.
  - Enter **Slave's Public IP & Select SSH credentials** (created in Step 3).
  - Select **Manually Trusted Key Verification Strategy**.
  - Set Availability to **Keep this agent online as much as possible**.
  - Save & Test Connection.

![Jenkins Slave Node Setup](../../images/jenkins_slave_setup1.png)

![Jenkins Slave Node Setup](../../images/jenkins_slave_setup2.png)
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
- **Pipeline: Stage View Plugin**
- **SonarQube Scanner Plugin**
- **Nexus Artifact Uploader**
- **Docker Plugin** (if using Docker)
- **SSH Build Agents** (for slave nodes)

---

## 🏁 Final Verification

### ✅ **Jenkins Master**
- Jenkins UI is accessible at `http://<JENKINS_MASTER_IP>:8080`
- Slave nodes are successfully connected.
- Required plugins are installed.

### ✅ **Jenkins Slave Nodes**
- Slave agents are running.
- Slave can execute build jobs.

![Jenkins Dashboard](../../images/jenkins_dashboard.png)

---

## 📄 Additional Documentation
- **[Jenkins Official Docs](https://www.jenkins.io/doc/)**
- **[Jenkins with AWS CodeDeploy](https://docs.aws.amazon.com/codedeploy/latest/userguide/integrating-jenkins-with-codedeploy.html)**

---

## 📧 Contact  
📧 **Email**: [sudarshangawande98@gmail.com](mailto:sudarshangawande98@gmail.com)  
🔗 **GitHub**: [Sudarshan Gawande](https://github.com/sudarshan-gawande)  
🌐 **Portfolio**: [sudarshangawande.com](https://sudarshangawande.com)  
💼 **LinkedIn**: [Sudarshan Gawande](https://www.linkedin.com/in/sudarshan-gawande/)  

---

## 📄 License
This project is licensed under the **MIT License**. See the `LICENSE` file for details.
