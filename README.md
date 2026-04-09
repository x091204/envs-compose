# Portable DevOps Tools Environment (Local Setup Guide)

## Purpose

This repository provides a **ready-to-run local DevOps environment** for students and learners.

It allows you to start working with commonly used CI/CD tools without installing each tool manually on your system.

Included services:

* Jenkins (preconfigured with DevOps tools)
* SonarQube (code quality scanner)
* PostgreSQL (SonarQube database backend)

Everything runs using Docker Compose.


# What This Setup Gives You

After starting the environment, you will have:

Jenkins running with:

* Docker CLI support
* Docker Compose
* Python3
* pip + virtual environment support
* Maven
* Trivy vulnerability scanner

SonarQube running with:

* persistent PostgreSQL database
* plugin storage
* scan history storage
* configuration persistence

This allows you to practice:

* CI/CD pipelines
* static code analysis
* container builds
* vulnerability scanning
* DevOps workflow automation

on your own machine.


# Requirements

Install the following before starting:

```
Docker
Docker Compose
```

Verify installation:

```
docker --version
docker compose version
```

---

# Folder Structure

```
envs-compose-main/

├── jenkins/
│   ├── Dockerfile
│   └── docker-compose.yml
│
├── sonarqube/
│   └── docker-compose.yml
```

Each service starts independently.


# Starting Jenkins

Open terminal:

```
cd jenkins
docker compose up -d
```

Access Jenkins:

```
http://localhost:8080
```

During first login:

1. Copy admin password from container logs

```
docker logs jenkins
```

2. Install suggested plugins


# Tools Available Inside Jenkins Container

This Jenkins image already includes:

```
Docker
Docker Compose
Python3
pip
virtualenv
Maven
Trivy
wget
gnupg
```

You do NOT need to install these manually.

They are ready for pipeline usage.

Example tasks supported:

```
Build project
Run tests
Scan code
Build Docker images
Run vulnerability scans
Deploy containers
```


# Starting SonarQube (with PostgreSQL)

Open terminal:

```
cd sonarqube
docker compose up -d
```

Access SonarQube:

```
http://localhost:9000
```

Default login:

```
username: admin
password: admin
```

You will be asked to change password after login.


# Why PostgreSQL Is Included

SonarQube requires a database to store:

* scan history
* project metrics
* vulnerabilities
* code smells
* authentication data
* plugin data

This setup already includes PostgreSQL so no additional configuration is required.


# Checking Running Containers

```
docker ps
```

Expected containers:

```
jenkins
sonarqube
postgres
```


# Stopping the Environment

Inside each service directory:

```
docker compose down
```


# Data Persistence

SonarQube data is stored using Docker volumes.

This means:

your scan history and configurations will remain even after container restart.


# Typical Learning Workflow Using This Setup

Example practice workflow:

```
Write application code
        ↓
Create Jenkins pipeline
        ↓
Run SonarQube scan
        ↓
Build Docker image
        ↓
Run Trivy scan
```

This environment is intended for local DevOps practice and experimentation.


# Notes

Make sure Docker service is running before starting containers.

If ports are already in use:

```
8080
9000
5432
```

stop the conflicting services or change ports inside docker-compose files.
