# ⚙️ Portable DevOps Environment

A ready-to-run local DevOps environment. Spin up Jenkins, SonarQube, and a full Kubernetes cluster in minutes — no manual installation needed.



## 📦 What's Inside

| Service | Tool | Purpose |
|---------|------|---------|
| Jenkins | Docker Compose | CI/CD pipelines |
| SonarQube + PostgreSQL | Docker Compose | Code quality analysis |
| Kind Kubernetes Cluster | Devbox + Task | Local K8s environment |



## 🗂️ Structure

```
envs-compose/
│
├── jenkins/
│   ├── Dockerfile              # Jenkins with pre-installed DevOps tools
│   └── docker-compose.yml      # Jenkins service
│
├── sonarqube/
│   └── docker-compose.yml      # SonarQube + PostgreSQL
│
└── kubernetes/
    ├── devbox.json             # All K8s tools via Devbox
    └── Cluster/
        ├── kind-config.yaml.TEMPLATE   # Template with path variables
        ├── kind-config.yaml            # Generated cluster config
        └── Taskfile.yml                # Automated cluster commands
```

**Jump to:**
- [Jenkins](#-jenkins)
- [SonarQube](#-sonarqube)
- [Kubernetes](#-kubernetes)
- [Tools included in Jenkins](#️-tools-included-in-jenkins)
- [Tools included in Devbox](#-tools-included-in-devbox)



## ✅ Requirements

```bash
# For Jenkins and SonarQube
docker --version
docker compose version

# For Kubernetes
devbox version   # https://www.jetify.com/devbox
task --version   # comes with devbox
kind --version   # comes with devbox
```



## 🔧 Jenkins

The Jenkins image comes pre-built with all the tools you need for a DevSecOps pipeline.

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
docker logs jenkins
```

Copy the password from the logs, paste it into the browser, then install suggested plugins.



## 🔍 SonarQube

Runs with a PostgreSQL backend so scan history and project data persist across restarts.

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



## ☸️ Kubernetes

A local multi-node Kind cluster with all Kubernetes tools pre-installed via Devbox. No manual tool installation needed — everything is declarative and reproducible.

### Step 1 — Install Devbox

```bash
curl -fsSL https://get.jetify.com/devbox | bash
```

### Step 2 — Enter the Devbox shell

```bash
cd kubernetes
devbox shell
```

This automatically installs and activates all tools listed in `devbox.json`.

### Step 3 — Create the cluster using Task

```bash
cd Cluster

# Generate the kind config with your local paths
task task.1:Generate-cluster-config

# Create the cluster
task task.2:Create-cluster
```

### Step 4 — Verify

```bash
kubectl get nodes
```

Expected output:
```
NAME                 STATUS   ROLES           AGE
kind-control-plane   Ready    control-plane   1m
kind-worker          Ready    <none>          1m
kind-worker2         Ready    <none>          1m
```

### Delete the cluster

```bash
task task.3:Delete-cluster
```



## 🛠️ Tools Included in Jenkins

Pre-installed in the custom Jenkins Dockerfile:

| Tool | Purpose |
|------|---------|
| Docker | Build and run containers inside pipelines |
| Docker Compose | Multi-container pipeline steps |
| Trivy | Container image vulnerability scanning |
| Maven | Java project build tool |
| Python3 + pip | Python project support |
| virtualenv | Python virtual environment support |
| wget, gnupg | Utility tools |



## 📦 Tools Included in Devbox

All installed automatically when you enter `devbox shell`:

| Tool | Purpose |
|------|---------|
| kubectl | Kubernetes CLI |
| kind | Local Kubernetes clusters in Docker |
| k9s | Terminal UI for Kubernetes |
| helm | Kubernetes package manager |
| kubectx | Switch between clusters and namespaces |
| kustomize | Kubernetes config management |
| jq | JSON processor |
| yq | YAML processor |
| gh | GitHub CLI |
| task | Task runner (Taskfile) |
| stern | Multi-pod log tailing |
| cloudflared | Cloudflare tunnel |
| mkcert | Local trusted certificates |
| kubeseal | Sealed Secrets for Kubernetes |
| envsubst | Environment variable substitution |
| oras | OCI registry artifact tool |



## 📋 Typical Workflow

```
Write code → push to GitHub
        ↓
Jenkins picks up the change
        ↓
SonarQube analysis (quality gate)
        ↓
Docker image build
        ↓
Trivy scan (HIGH/CRITICAL check)
        ↓
Push image to Docker Hub
        ↓
Deploy to Kind cluster with kubectl or helm
```



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

# Tail logs from multiple pods
stern -n your-namespace .

# Switch namespace
kubens your-namespace

# Open k9s dashboard
k9s
```



## ⚠️ Port Conflicts

| Port | Service |
|------|---------|
| 8080 | Jenkins |
| 9000 | SonarQube |
| 5432 | PostgreSQL |

Stop conflicting services or update port mappings in the relevant `docker-compose.yml`.



## 💾 Data Persistence

| Volume | Stores |
|--------|--------|
| `jenkins_home` | Jenkins jobs, configs, plugins |
| `sonarqube_data` | Scan results, metrics |
| `sonarqube_extensions` | SonarQube plugins |
| `sonarqube_logs` | SonarQube logs |
| `postgresql_data` | SonarQube database |

To fully wipe everything:

```bash
docker compose down -v
```


## 📄 License

MIT
