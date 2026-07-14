# 🚀 Deploying Node.js on Amazon EKS with Helm

[![GitHub repo size](https://img.shields.io/github/repo-size/atulupadhyay2004/EKS)](https://github.com/atulupadhyay2004/EKS)
[![GitHub stars](https://img.shields.io/github/stars/atulupadhyay2004/EKS?style=social)](https://github.com/atulupadhyay2004/EKS/stargazers)

> A complete end-to-end guide to deploying a Node.js application on **Amazon EKS** using **Helm**, from cluster provisioning to application rollout.

---

## 📋 Table of Contents
- [About The Project](#-about-the-project)
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [System Architecture](#-system-architecture)
- [Workflow](#-workflow)
- [Project Structure](#-project-structure)
- [Helm Chart Overview](#-helm-chart-overview)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Clone the Repository](#clone-the-repository)
  - [Provision the EKS Cluster](#provision-the-eks-cluster)
  - [Build and Push the Docker Image](#build-and-push-the-docker-image)
  - [Deploy with Helm](#deploy-with-helm)
  - [Access the Application](#access-the-application)
- [Cleanup](#-cleanup)
- [Contributing](#-contributing)
- [License](#-license)
- [Contact](#-contact)

---

## 📖 About The Project

This project demonstrates a **production-ready deployment pipeline** for a Node.js application on **Amazon Elastic Kubernetes Service (EKS)**. It includes:

- **EKS cluster provisioning** using `eksctl` with a managed node group
- A **Node.js** application with `/` and `/health` endpoints
- **Docker** containerization of the application
- **Helm chart** for templated Kubernetes deployments
- **Zero-downtime rolling updates** with liveness and readiness probes

The goal is to provide a complete, reusable template for deploying containerized applications on AWS EKS using industry-standard tools.

---

## ✨ Features

- ✅ **EKS cluster** — Provisioned with `eksctl` in `ap-south-1` (Mumbai) region
- ✅ **Managed node group** — `t3.micro` instances with auto-scaling (1-2 nodes)
- ✅ **Node.js application** — Express.js app with health check endpoint
- ✅ **Dockerized** — Multi-stage Dockerfile for efficient builds
- ✅ **Helm chart** — Parameterized Kubernetes manifests
- ✅ **Health probes** — Liveness and readiness checks for pod stability
- ✅ **Rolling updates** — Zero-downtime deployments
- ✅ **Pod identity** — Each pod reports its own hostname

---

## 🛠 Tech Stack

| Category | Technology |
|----------|------------|
| **Cloud Provider** | Amazon Web Services (EKS) |
| **Cluster Provisioning** | `eksctl` |
| **Orchestration** | Kubernetes (EKS) |
| **Package Manager** | Helm (v3) |
| **Application** | Node.js (Express) |
| **Containerization** | Docker |
| **Registry** | Docker Hub / Amazon ECR |
| **Instance Type** | t3.micro |
| **Region** | ap-south-1 (Mumbai) |

---

---

## 🔄 Workflow

This diagram illustrates the end-to-end deployment workflow:

```mermaid
flowchart TD
    A[Developer writes Node.js app] --> B[Build Docker image]
    B --> C[Push image to registry]
    C --> D[Update Helm chart values]
    
    E[Developer defines cluster-config.yaml] --> F[eksctl create cluster]
    F --> G[EKS cluster is provisioned]
    G --> H[kubectl config updated]
    
    D --> I[helm install/upgrade nodejs-app]
    H --> I
    I --> J[Helm renders templates]
    J --> K[Kubernetes API receives manifests]
    K --> L[Deployment creates pods]
    L --> M[Pods pull image from registry]
    M --> N[Liveness & Readiness probes check /health]
    N --> O{All pods healthy?}
    O -->|Yes| P[Application is live on EKS]
    O -->|No| Q[Rolling update pauses]
    Q --> R[Old pods remain running]
    R --> S[Investigate and fix issues]
    S --> I
