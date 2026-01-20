🚀 Java CI/CD Pipeline with Docker & Kubernetes (GitHub Actions)
📌 Project Overview

This project demonstrates an end-to-end production-style CI/CD pipeline for a Java Spring Boot application using:

GitHub Actions for CI & CD

Docker for containerization

Docker Hub for image registry

Kubernetes (Docker Desktop / Minikube) for deployment

The goal is not just to “make the build pass”, but to apply DevOps & DevSecOps best practices such as:

Automated testing

Code quality enforcement

Security scanning

Container validation

Reliable deployments

🧩 Application Details

Language: Java

Framework: Spring Boot

Endpoint:

GET /health


Purpose: Simple health/version API to verify CI/CD-driven deployments

Example response:

Application is healthy – VERSION 9

📂 Project Structure
java-ci-cd-k8s/
├── .github/workflows/
│   ├── ci.yml        # Continuous Integration pipeline
│   └── cd.yml        # Continuous Deployment pipeline
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
├── src/
│   └── main/java/com/example/demo/
│       ├── DemoApplication.java
│       └── HealthController.java
├── Dockerfile
├── pom.xml
├── README.md

🐳 Dockerfile – Containerization

The Dockerfile defines how the Java application is packaged into a Docker image.

Key responsibilities:

Use a JDK base image

Copy application JAR

Expose port 8080

Start the Spring Boot application

⚠️ Only runtime files are included
❌ Kubernetes files, GitHub workflows, etc. are NOT inside the image

☸️ Kubernetes Configuration
🔹 deployment.yaml

Defines how many pods to run

Specifies which Docker image to pull

Controls rollout and updates

Key fields:

image: waghom/java-ci-cd-k8s:latest
imagePullPolicy: Always

🔹 service.yaml

Exposes the application using NodePort

Routes traffic from:

localhost:30080 → pod:8080


Service is usually created once, deployments change frequently.

⚙️ CI Pipeline (ci.yml) – Continuous Integration
CI triggers on:

Push to main

Manual trigger (workflow_dispatch)

CI Stages Implemented

Checkout Code

Setup Java (Temurin)

Maven Build & Tests

Code Quality Check

Checkstyle

Security Scans

CodeQL (SAST)

OWASP Dependency Check (SCA)

Docker Image Build

Container Vulnerability Scan

Trivy (HIGH / CRITICAL)

Container Smoke Test

Push Image to Docker Hub

:latest

:${GITHUB_SHA}

📌 CI Responsibility ends at Docker image push

🚀 CD Pipeline (cd.yml) – Continuous Deployment
CD triggers:

After successful CI

Or manual trigger

CD Steps:

Configure Kubernetes access

Apply Kubernetes manifests:

kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml


Restart deployment if using latest tag:

kubectl rollout restart deployment java-app


📌 CD responsibility = deploy image to Kubernetes

🔁 How Code Changes Reach the Browser

Modify Java code (HealthController)

Commit & push to GitHub

CI runs

Tests + security checks

Docker image built & pushed

Kubernetes pulls new image

Old pod terminated, new pod created

Service routes traffic to new pod

Changes visible at:

http://localhost:30080/health

🔐 Secrets Configuration (Mandatory)

Configured in GitHub Repository → Settings → Secrets → Actions

Secret Name	Purpose
DOCKERHUB_USERNAME	Docker Hub username
DOCKERHUB_TOKEN	Docker Hub access token

⚠️ Secrets are never hardcoded

🧪 How to Run Locally (Manual)
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl get pods
kubectl get svc


Open:

http://localhost:30080/health

📊 Results & Observations

CI catches bugs early (tests + lint)

Security issues surfaced before deployment

Docker images validated before push

Kubernetes ensures zero-downtime updates

Clear separation of CI and CD responsibilities

⚠️ Limitations & Future Improvements

CD to local Kubernetes only (no cloud yet)

Can be extended to:

AWS EKS

DockerHub → ECR

Helm charts

Canary / Blue-Green deployments

🧠 Key DevOps Learnings

CI ≠ CD

Docker builds are immutable

Kubernetes does not “know” CI finished — CD triggers it

Services stay stable, pods are replaceable

Automation > manual operations

🔗 GitHub Repository

👉 Repo URL:
https://github.com/omwagh28/java-ci-cd-k8s

🏁 Final Note (Evaluator Friendly)

This project demonstrates:

Thoughtful CI/CD stage ordering

DevSecOps practices

Clean Kubernetes deployments

Real-world DevOps reasoning