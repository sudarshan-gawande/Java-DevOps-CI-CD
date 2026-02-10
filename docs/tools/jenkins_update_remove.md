# Jenkins Update and Removal Guide (RHEL/CentOS/Amazon Linux)

This document provides comprehensive commands to update, troubleshoot, and completely remove Jenkins from a system using `dnf` or `yum` on RHEL-based systems.

---

## 📋 Table of Contents
1. [Jenkins Update](#-jenkins-update)
2. [Jenkins Service Management](#-jenkins-service-management)
3. [Jenkins Removal (Complete Clean Uninstall)](#-jenkins-removal-complete-clean-uninstall)
4. [Troubleshooting](#-troubleshooting)
5. [Verification](#-verification)

---

## 🔄 Jenkins Update

### **1. Update Repository Metadata**
```bash
sudo yum update -y
```

### **2. Check Available Jenkins Updates**
```bash
sudo yum check-update jenkins
```

### **3. Upgrade Jenkins Package**
```bash
sudo yum upgrade jenkins -y
```

### **4. Install Specific Jenkins Version (Optional)**
```bash
# List available versions
sudo yum list available jenkins

# Install specific version
sudo yum install jenkins-<version> -y
```

### **5. Reload systemd and Restart Jenkins**
```bash
sudo systemctl daemon-reload
sudo systemctl restart jenkins
```

### **6. Verify Jenkins Status**
```bash
sudo systemctl status jenkins
```

### **7. Check Jenkins Version**
```bash
jenkins --version
```

### ⚠️ **Pre-Update Recommendations**
- Backup Jenkins configuration and jobs before updating
- Create a snapshot/AMI of your Jenkins instance
- Test updates in a non-production environment first
- Schedule updates during maintenance windows

---

## 🔧 Jenkins Service Management

### **Start Jenkins**
```bash
sudo systemctl start jenkins
```

### **Stop Jenkins**
```bash
sudo systemctl stop jenkins
```

### **Restart Jenkins**
```bash
sudo systemctl restart jenkins
```

### **Enable Jenkins on Boot**
```bash
sudo systemctl enable jenkins
```

### **Disable Jenkins on Boot**
```bash
sudo systemctl disable jenkins
```

### **Check Jenkins Logs**
```bash
sudo tail -f /var/log/jenkins/jenkins.log
```

### **Check Service Status with Details**
```bash
sudo systemctl status jenkins -l
```

---

## 🧹 Jenkins Removal (Complete Clean Uninstall)

### **⚠️ WARNING**
This process **permanently removes** all jobs, configurations, plugins, build history, and Jenkins data. Ensure you have backups before proceeding.

### **Step 1: Stop and Disable Service**
```bash
sudo systemctl stop jenkins
sudo systemctl disable jenkins
```

### **Step 2: Remove Jenkins Package**
```bash
sudo yum remove jenkins -y
```

### **Step 3: Remove Jenkins Repository File**
```bash
sudo rm -f /etc/yum.repos.d/jenkins.repo
```

### **Step 4: Delete Jenkins Data, Logs, and Cache**
```bash
# Remove Jenkins home directory (contains all jobs and configurations)
sudo rm -rf /var/lib/jenkins

# Remove Jenkins logs
sudo rm -rf /var/log/jenkins

# Remove Jenkins cache
sudo rm -rf /var/cache/jenkins
```

### **Step 5: Remove Jenkins User and Group (Optional)**
```bash
# Remove Jenkins user
sudo userdel -r jenkins

# Remove Jenkins group (if it still exists)
sudo groupdel jenkins
```

### **Step 6: Remove Jenkins Plugins Directory (if separate)**
```bash
sudo rm -rf ~/.jenkins/plugins/
```

### **Step 7: Reload systemd**
```bash
sudo systemctl daemon-reload
```

### **Step 8: Clean Package Cache**
```bash
sudo yum clean all
```

---

## 🧪 Troubleshooting

### **Jenkins Won't Start**
```bash
# Check service status
sudo systemctl status jenkins

# View detailed logs
sudo journalctl -u jenkins -n 100 --no-pager

# Check for port conflicts (8080)
sudo netstat -tulpn | grep :8080
```

### **Jenkins Port Already in Use**
```bash
# Find process using port 8080
sudo lsof -i :8080

# Kill the process (if needed)
sudo kill -9 <PID>
```

### **Permission Issues**
```bash
# Fix Jenkins directory permissions
sudo chown -R jenkins:jenkins /var/lib/jenkins
sudo chmod -R 755 /var/lib/jenkins
```

### **Java Version Mismatch**
```bash
# Check Java version
java -version

# List installed Java versions
sudo update-alternatives --list java

# Switch Java version
sudo update-alternatives --config java
```

### **Clear Jenkins Cache and Rebuild**
```bash
sudo systemctl stop jenkins
sudo rm -rf /var/cache/jenkins
sudo systemctl start jenkins
```

---

## 🗂️ Jenkins Backup and Restore

### **Backup Jenkins Configuration**
```bash
# Create backup directory
mkdir -p ~/jenkins-backups

# Backup Jenkins home directory
sudo tar -czf ~/jenkins-backups/jenkins-backup-$(date +%Y%m%d-%H%M%S).tar.gz /var/lib/jenkins
```

### **Restore Jenkins Configuration**
```bash
# Stop Jenkins
sudo systemctl stop jenkins

# Restore from backup
sudo tar -xzf ~/jenkins-backups/jenkins-backup-YYYYMMDD-HHMMSS.tar.gz -C /

# Restart Jenkins
sudo systemctl start jenkins
```

---

## 🗑️ Optional --- Remove Java

### **Remove Java 21**
```bash
sudo yum remove java-21-openjdk-headless -y
```

### **Remove All Java Versions**
```bash
sudo yum remove java-* -y
```

### **Verify Java Removal**
```bash
java -version
```

Expected output:
```
command not found
```

---

## ✅ Verification

### **Verify Jenkins Removal**
```bash
jenkins --version
```

Expected output:
```
command not found
```

### **Verify Repository Removed**
```bash
ls /etc/yum.repos.d/jenkins.repo
```

Expected output:
```
ls: cannot access '/etc/yum.repos.d/jenkins.repo': No such file or directory
```

### **Verify Jenkins User Removed**
```bash
id jenkins
```

Expected output:
```
id: 'jenkins': no such user
```

### **Verify Port 8080 is Free**
```bash
sudo netstat -tulpn | grep :8080
```

Expected output:
```
(empty)
```

---

## 📋 Summary Checklist

- [ ] Backed up Jenkins configuration and jobs
- [ ] Stopped Jenkins service
- [ ] Removed Jenkins package
- [ ] Deleted Jenkins data directories
- [ ] Removed Jenkins user/group
- [ ] Cleaned repository files
- [ ] Verified complete removal
- [ ] Removed Java (if needed)

---

## 📄 Additional Documentation
- **[Jenkins Official Docs](https://www.jenkins.io/doc/)**
- **[Jenkins Upgrade Guide](https://www.jenkins.io/doc/upgrade-guide/)**
- **[RHEL/CentOS Package Management](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/)**

---

## 📧 Contact  
📧 **Email**: [sudarshangawande98@gmail.com](mailto:sudarshangawande98@gmail.com)  
🔗 **GitHub**: [Sudarshan Gawande](https://github.com/sudarshan-gawande)  
🌐 **Portfolio**: [sudarshangawande.com](https://sudarshangawande.com)  
💼 **LinkedIn**: [Sudarshan Gawande](https://www.linkedin.com/in/sudarshan-gawande/)  

---

## 📄 License
This project is licensed under the **MIT License**. See the `LICENSE` file for details.