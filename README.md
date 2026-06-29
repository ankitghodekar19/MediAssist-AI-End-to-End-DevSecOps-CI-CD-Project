# 🚀 MediAssist-AI

## Enterprise DevSecOps CI/CD Pipeline for an AI-Powered Healthcare Application

![Jenkins](https://img.shields.io/badge/Jenkins-CI/CD-red?logo=jenkins)
![Docker](https://img.shields.io/badge/Docker-Containerization-blue?logo=docker)
![SonarQube](https://img.shields.io/badge/SonarQube-Code%20Quality-4E9BCD?logo=sonarqube)
![OWASP](https://img.shields.io/badge/OWASP-Dependency%20Check-orange)
![Trivy](https://img.shields.io/badge/Trivy-Vulnerability%20Scanner-blueviolet)
![Node.js](https://img.shields.io/badge/Node.js-Backend-green?logo=node.js)
![FastAPI](https://img.shields.io/badge/FastAPI-AI%20Service-009688?logo=fastapi)
![MongoDB](https://img.shields.io/badge/MongoDB-Database-47A248?logo=mongodb)
![License](https://img.shields.io/badge/License-MIT-green)

---

# 📖 Overview

MediAssist-AI is an AI-powered healthcare application built with a complete Enterprise DevSecOps CI/CD pipeline.

The project demonstrates how a modern software application is automatically built, tested, scanned for vulnerabilities, containerized, deployed, and monitored using industry-standard DevOps tools.

The pipeline integrates security at every stage by performing static code analysis, dependency vulnerability scanning, filesystem scanning, container image scanning, automated Docker image creation, deployment, health verification, and email notifications.

This project closely resembles production-grade DevSecOps workflows used in enterprise environments.

---

# 🏗 Architecture

> *(Architecture Diagram will be added here.)*

---

# ⚙ Technology Stack

## Frontend

- HTML
- CSS
- JavaScript
- Node.js

## Backend

- Node.js
- Express.js

## AI Service

- Python
- FastAPI

## Database

- MongoDB

## DevOps Tools

- Jenkins
- Docker
- Docker Hub
- SonarQube
- OWASP Dependency Check
- Trivy
- Gmail SMTP

---

# ✨ Features

- AI-powered disease prediction
- Microservice architecture
- Dockerized application
- Automated CI/CD pipeline
- Static code quality analysis
- Dependency vulnerability scanning
- Filesystem vulnerability scanning
- Docker image security scanning
- Automated Docker image publishing
- Health monitoring
- Email notifications
- Artifact archiving

---

# 🚀 CI/CD Pipeline

The Jenkins pipeline performs the following stages automatically.

- Checkout Source Code
- SonarQube Static Code Analysis
- SonarQube Quality Gate Validation
- OWASP Dependency Check
- Publish Dependency Check Reports
- Trivy Filesystem Scan
- Build Backend Docker Image
- Build Frontend Docker Image
- Build AI Docker Image
- Trivy Docker Image Scan
- Publish Trivy Reports
- Docker Hub Login
- Push Docker Images
- Deploy Application Containers
- Health Check
- Archive Reports
- Email Notification

---

# 🔒 Security Implementation

## SonarQube

Performs static code analysis to detect:

- Bugs
- Code Smells
- Vulnerabilities
- Security Hotspots
- Maintainability Issues

---

## OWASP Dependency Check

Scans project dependencies against the National Vulnerability Database (NVD).

Generated Reports:

- HTML Report
- XML Report

---

## Trivy Filesystem Scan

Scans project source code before Docker image creation.

Checks for:

- Vulnerabilities
- Secrets
- Misconfigurations

---

## Trivy Docker Image Scan

Scans every Docker image before deployment.

Images scanned:

- Backend
- Frontend
- AI Service

---

# 🐳 Docker Containers

| Service | Technology | Port |
|----------|------------|------|
| Frontend | Node.js | 8000 |
| Backend | Express.js | 3000 |
| AI Service | FastAPI | 5000 |
| MongoDB | MongoDB | 27017 |

---

# 📦 Docker Images

Docker Hub Repository

```
ankitghodekar/health-frontend

ankitghodekar/health-backend

ankitghodekar/health-ai
```

---

# 📂 Project Structure

```
MediAssist-AI
│
├── AI
│
├── backend
│
├── frontend
│
├── docs
│
├── Jenkinsfile
│
├── docker-compose.yml
│
├── README.md
│
└── reports
```

---

# ▶ Running Locally

Clone Repository

```bash
git clone https://github.com/ankitghodekar19/MediAssist-AI.git
```

Go to project

```bash
cd MediAssist-AI
```

Run Docker Compose

```bash
docker compose up -d
```

Verify Containers

```bash
docker ps
```

---

# 🌐 Application URLs

| Service | URL |
|----------|-----|
| Frontend | http://localhost:8000 |
| Backend Health | http://localhost:3000/health |
| AI Service | http://localhost:5000 |
| SonarQube | http://localhost:9000 |

---

# 📊 Reports

The pipeline automatically generates the following reports.

- SonarQube Analysis Report
- OWASP Dependency Check Report
- Trivy Filesystem Scan Report
- Trivy Image Scan Reports
- Jenkins Build Artifacts

---

# 📷 Screenshots

> Screenshots will be added here.


### Jenkins Pipeline

![Jenkins Pipeline](docs/jenkins-pipeline.png)


---

---

# 📧 Email Notification

The pipeline automatically sends an email after every execution containing:

- Build Status
- Build Number
- Pipeline Result
- Jenkins Build URL
- Console Log Link

---

# 🚀 Future Improvements

- Kubernetes Deployment
- Helm Charts
- Prometheus Monitoring
- Grafana Dashboards
- ArgoCD GitOps
- AWS EC2 Deployment
- Terraform Infrastructure as Code
- Ansible Automation
- HashiCorp Vault Integration

---

# 👨‍💻 Author

**Ankit Ghodekar**

DevOps Engineer

GitHub: https://github.com/ankitghodekar19

LinkedIn: www.linkedin.com/in/ankit-ghodekar-112baa25b

---

## ⭐ Support

If you found this project useful, consider giving it a **Star ⭐** on GitHub.

It helps others discover the project and supports my work.