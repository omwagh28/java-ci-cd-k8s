# Java CI/CD Pipeline using GitHub Actions, Docker & Kubernetes

## 📌 Project Overview
This project implements a **complete CI/CD pipeline** for a Java Spring Boot application using **GitHub Actions**, **Docker**, and **Kubernetes (local cluster)**.

The goal is to demonstrate how modern DevOps practices automate:
- Build
- Test
- Containerization
- Deployment

with minimal manual intervention and high reliability.

---

## 🎯 Problem Background & Motivation
Traditional manual deployments are:
- Error-prone
- Time-consuming
- Not scalable

As applications grow, teams need a reliable way to:
- Automatically validate code
- Ensure consistent builds
- Deploy changes safely

This project solves these problems by introducing an **automated CI/CD pipeline** where every code change goes through standardized stages before deployment.

---

## 🧩 Application Overview
- **Language:** Java  
- **Framework:** Spring Boot  
- **Endpoint:** `/health`  
- **Purpose:** Display application version to verify deployment changes  


---

## 🏗️ CI/CD Architecture (High Level)

Developer
|
| git push
v
GitHub Repository
|
| GitHub Actions (CI)
v
Docker Image Build
|
| Push Image
v
DockerHub
|
| GitHub Actions (CD)
v
Kubernetes Deployment
|
v
Kubernetes Service (NodePort)
|
v
Browser (localhost)


---

## 🔁 Continuous Integration (CI)

### Trigger
- Push to `main` branch
- Manual workflow trigger

### CI Stages
1. Checkout source code
2. Setup Java & Maven
3. Run unit tests
4. Build application JAR
5. Build Docker image using Dockerfile
6. Push Docker image to DockerHub

CI ensures:
- Code quality
- Build consistency
- Early failure detection

---

## 🚀 Continuous Deployment (CD)

### CD Responsibilities
1. Pull latest Docker image
2. Apply Kubernetes deployment
3. Perform rolling update of pods
4. Keep service endpoint stable

Deployment updates occur **without downtime**.

---

## 🐳 Dockerfile Role
The Dockerfile defines:
- Base Java runtime
- Application JAR inclusion
- Container startup command

It ensures the application runs identically across environments.

---

## ☸️ Kubernetes Role

### Deployment
- Manages pods
- Handles rolling updates
- Ensures desired replica count

### Service
- Exposes application via NodePort
- Routes traffic to active pods
- Keeps endpoint stable while pods change

---

## 🔐 Security & Quality Controls
- DockerHub credentials stored as GitHub Secrets
- No secrets hardcoded in repository
- Pipeline fails on build/test errors
- Controlled image pull using `imagePullPolicy`

---

## 🖥️ How to Run Locally

### Prerequisites
- Docker Desktop
- Kubernetes enabled in Docker Desktop
- kubectl installed

### Steps
```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

Access the application:
http://localhost:30080/health


🔑 GitHub Secrets Configuration

Configured in:
GitHub Repository → Settings → Secrets → Actions

Secret Name	Purpose
DOCKERHUB_USERNAME	DockerHub login
DOCKERHUB_TOKEN	DockerHub access token
📊 Results & Observations

Automated deployments reduce human error

Kubernetes ensures zero-downtime updates

CI/CD improves development speed

Clear separation of build and deploy responsibilities

⚠️ Limitations & Future Improvements
Current Limitations

Local Kubernetes cluster only

No cloud load balancer

No monitoring stack

Future Improvements

Deploy to AWS EKS

Use Ingress instead of NodePort

Add security scans (Trivy, CodeQL)

Add monitoring (Prometheus, Grafana)

🏁 Conclusion

This project demonstrates how CI/CD pipelines improve software delivery by combining automation, containerization, and orchestration using modern DevOps tools.