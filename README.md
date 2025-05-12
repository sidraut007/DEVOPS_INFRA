## 🛠 DEVOPS_INFRA Setup (Jenkins + SonarQube + Nexus)

This guide explains how to set up a full DevOps CI/CD pipeline using **Jenkins**, **SonarQube**, and **Nexus** with Docker. 
---

## 📦 Step-by-Step Setup

### 1. 🔧 Clone the Repository

```bash
git clone https://github.com/sidraut007/DEVOPS_INFRA.git

cd DEVOPS_INFRA/
```
---

### 2. 🐳 Start the Infrastructure with Docker Compose

```bash
cd DEVOPS_INFRA/CICD_tool

docker-compose up -d

```

- This will:
    - Run Jenkins at:    http://localhost:8080
    - Run Sonar at:    http://localhost:9000
    - Run Nexus at:    http://localhost:8081

---

### 3. 👷 Configure Jenkins

- Install suggested plugins, then install:
	1. SonarQube Scanner
	2. Config File Provider
	3. Maven Integration
	4. Pipeline Maven Integration
	5. Kubernetes Credentials
	6. Kubernetes
	7. Kubernetes CLI
	8. Kubernetes Client API
	9. Docker Pipeline

- Configure global tools under Manage Jenkins → Global Tool Configuration:
    - Maven, Sonar-Scanner, 

- Add required credentials:

    - GitHub (Personal Access Token)
    - DockerHub (username/password)
    - SonarQube token
    - Nexus user credentials #in global maven setting

---

### 4. ⚙️ Jenkins CI Pipeline 
- write complete jenkis declarative pipeline 

    - Add your Jenkinsfile in the root of the repo.
    - Set SCM to Git, using your repository URL.

---

### 5. 🧪 Configure SonarQube
- Login to SonarQube as admin/admin

- Configure a webhook under:
    - Project Settings → General → Webhooks
    
    ```
    http://jenkins:8080/sonar-webhook/
    ```

- Test Webhook from Sonar Container
    ```
    docker exec -it <sonar-container-name> curl -I http://jenkins:8080/sonar-webhook/
    ```

---

### 6. 📦 Nexus Repository Setup
- Login to Nexus at http://localhost:8081 as admin.
- Create a Maven hosted repository (e.g., maven-releases).
- Add this to your pom.xml:

```bash

    <distributionManagement>
    <repository>
        <id>nexus</id>
        <url>http://nexus:8081/repository/maven-releases/</url>
    </repository>
    </distributionManagement>

    <distributionManagement>
    <repository>
        <id>nexus</id>
        <url>http://nexus:8081/repository/maven-snapshots/</url>
    </repository>
    </distributionManagement>

```
