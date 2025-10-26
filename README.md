## 🚀 **1. Overview: What Jenkins Is**

<img width="1183" height="616" alt="1723117995238" src="https://github.com/user-attachments/assets/00ca87f8-6802-47e6-b43b-74353d71488d" />


**Jenkins** is an open-source automation server widely used for **Continuous Integration (CI)** and **Continuous Delivery (CD)** in DevOps pipelines. It helps automate:

* Code building
* Testing
* Deployment
* Monitoring

You can integrate Jenkins with Git, Docker, Kubernetes, Maven, Ansible, Terraform, AWS, Azure, and more.

---

## 🧰 **2. Prerequisites**

### Hardware/Software Requirements

| Component | Minimum              | Recommended   |
| --------- | -------------------- | ------------- |
| CPU       | 2 cores              | 4 cores       |
| RAM       | 2 GB                 | 4–8 GB        |
| Disk      | 10 GB                | 50+ GB        |
| OS        | Ubuntu/Debian/CentOS | Ubuntu 22.04+ |
| Java      | JDK 11 or JDK 17     | JDK 17        |

---

## ⚙️ **3. Installation Steps**

### **A. Install Java**

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
java -version
```

### **B. Install Jenkins**

```bash
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

Access Jenkins at:
👉 `http://<your-server-ip>:8080`

---

## 🔑 **4. Initial Setup**

1. Run `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
2. Paste the password in the browser UI.
3. Choose **“Install suggested plugins.”**
4. Create your **Admin user**.
5. Set Jenkins URL.

---

## 🧩 **5. Jenkins Configuration**

### **Install Common Plugins**

* Git
* Pipeline
* Blue Ocean
* Docker Pipeline
* Kubernetes
* Ansible
* Email Extension
* Slack Notification
* SonarQube Scanner

**Manage Jenkins → Manage Plugins → Available → Search & Install**

---

## 🧠 **6. Integrations**

### **A. Source Code Management (SCM)**

* Integrate Jenkins with GitHub, GitLab, or Bitbucket.

  * Add Git credentials under:
    `Manage Jenkins → Credentials → Global → Add Credentials`
  * Use repository URL in your pipeline.

### **B. Build Tools**

* **Maven:** `sudo apt install maven -y`
* **Gradle:** `sudo apt install gradle -y`

Add in Jenkins:
`Manage Jenkins → Global Tool Configuration`

### **C. Containerization (Docker)**

```bash
sudo apt install docker.io -y
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

Install “Docker Pipeline” plugin and use docker agents in your pipelines.

### **D. Infrastructure as Code**

Integrate:

* **Ansible** → for configuration management
* **Terraform** → for provisioning
  Add relevant credentials and scripts in Jenkins jobs.

### **E. Cloud Integration**

Use plugins for:

* AWS (S3, EC2, ECR, CodeDeploy)
* Azure
* GCP

---

## 🧾 **7. Pipeline Setup (Jenkinsfile)**

Example of a simple CI/CD pipeline for a Java app:

```groovy
pipeline {
    agent any
    tools {
        maven 'Maven'
        jdk 'JDK17'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/user/repo.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t myapp:latest .'
            }
        }
        stage('Deploy') {
            steps {
                sh './deploy.sh'
            }
        }
    }
    post {
        success {
            echo 'Build and Deployment Successful!'
        }
        failure {
            echo 'Build Failed!'
        }
    }
}
```

---

## 🧪 **8. CI/CD Example Flow**

<img width="800" height="400" alt="jenkins-features-diagram" src="https://github.com/user-attachments/assets/b687baa1-1bbf-415e-b237-e5aea279dd14" />

1. Developer commits code → GitHub Webhook triggers Jenkins.
2. Jenkins builds, tests, and packages the app.
3. Docker image created and pushed to registry (DockerHub/ECR).
4. Deployment triggered to staging/prod using Ansible or Kubernetes.

---

## 📊 **9. Monitoring & Reporting**

* Integrate with **Prometheus + Grafana** or **ELK Stack**.
* Use plugins like:

  * Build History Metrics
  * Monitoring
  * Performance Publisher

---

## 🛡️ **10. Security Best Practices**

* Enable Role-Based Access Control (RBAC)
* Integrate LDAP/AD or OAuth
* Use HTTPS (SSL/TLS)
* Regularly update Jenkins and plugins
* Run Jenkins as a non-root user

---

## 🧩 **11. Backup & Recovery**

* Backup `/var/lib/jenkins`
* Use plugins like “ThinBackup”
* Store backups in S3 or NFS

---

## 🌐 **12. Common Jenkins Setup Architecture**

<img width="1186" height="495" alt="cicd-jenkins-rafay" src="https://github.com/user-attachments/assets/4d2ce1ea-bc45-4abc-8e04-6d3da77e0e62" />   <img width="1012" height="1034" alt="jenkins-dataflow" src="https://github.com/user-attachments/assets/79b72f8e-6f9e-4e62-ad31-a0de28f9ce38" />


**Example:**

```
GitHub → Jenkins → Docker → Kubernetes (EKS/GKE/AKS)
                   ↓
              SonarQube / Nexus
                   ↓
            Prometheus + Grafana
```

---
