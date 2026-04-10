# ⚙️ Portable DevOps Environment

A ready-to-run local DevOps environment using Docker Compose. Spin up Jenkins and SonarQube in minutes — no manual installation needed

## 📦 What's Inside

| Service | Image | Port | Purpose |
|---------|-------|------|---------|
| Jenkins | Custom (LTS + tools) | 8080 | CI/CD pipelines |
| SonarQube | sonarqube:10.4-community | 9000 | Code quality analysis |
| PostgreSQL | postgres:15 | 5432 | SonarQube database backend |


## 🗂️ Structure

```
envs-compose/
├── jenkins/
│   ├── Dockerfile          # Jenkins with pre-installed DevOps tools
│   └── docker-compose.yml  # Jenkins service
│
└── sonarqube/
    └── docker-compose.yml  # SonarQube + PostgreSQL
```

**Jump to:**
- [Jenkins setup](#-jenkins)
- [SonarQube setup](#-sonarqube)
- [Tools included in Jenkins](#️-tools-included-in-jenkins)


## ✅ Requirements

```bash
docker --version
docker compose version
```

Both must be installed before starting.


## 🔧 Jenkins

The Jenkins image comes pre-built with all the tools you need for a DevSecOps pipeline. No manual plugin installation required for the core tools.

### Start

```bash
cd jenkins
docker compose up -d
```

### Access

```
http://localhost:8080
```

### First login

```bash
# Get the admin password
docker logs jenkins
```

Copy the password from the logs, paste it into the browser, then install suggested plugins.


## 🔍 SonarQube

SonarQube runs with a PostgreSQL backend so your scan history and project data persist across restarts.

### Start

```bash
cd sonarqube
docker compose up -d
```

### Access

```
http://localhost:9000
```

### Default credentials

```
username: admin
password: admin
```

You will be prompted to change the password on first login.


## 🛠️ Tools Included in Jenkins

The custom Jenkins Dockerfile pre-installs everything you need:

| Tool | Purpose |
|------|---------|
| Docker | Build and run containers inside pipelines |
| Docker Compose | Multi-container pipeline steps |
| Trivy | Container image vulnerability scanning |
| Maven | Java project build tool |
| Python3 + pip | Python project support |
| virtualenv | Python virtual environment support |
| wget, gnupg | Utility tools |

You do not need to install any of these manually — they are ready inside the container.


## 📋 Typical Workflow

Write code → push to GitHub
        ↓
Jenkins picks up the change
        ↓
SonarQube analysis (code quality gate)
        ↓
Docker image build
        ↓
Trivy scan (HIGH/CRITICAL vulnerabilities)
        ↓
Push image to Docker Hub
```

---

## 🛑 Useful Commands

```bash
# Check running containers
docker ps

# View Jenkins logs
docker logs jenkins

# View SonarQube logs
docker logs sonarq

# Stop Jenkins
cd jenkins && docker compose down

# Stop SonarQube
cd sonarqube && docker compose down
```


## ⚠️ Port Conflicts

If you see port errors on startup, these ports must be free:

| Port | Service |
|------|---------|
| 8080 | Jenkins |
| 9000 | SonarQube |
| 5432 | PostgreSQL |

Stop any conflicting services or change the port mappings in the relevant `docker-compose.yml` file.


## 💾 Data Persistence

All data is stored in Docker named volumes:

| Volume | Stores |
|--------|--------|
| `jenkins_home` | Jenkins jobs, configs, plugins |
| `sonarqube_data` | Scan results, metrics |
| `sonarqube_extensions` | SonarQube plugins |
| `sonarqube_logs` | SonarQube logs |
| `postgresql_data` | SonarQube database |

Your data survives container restarts. To fully wipe everything:

```bash
docker compose down -v
```


## 📄 License

MIT
