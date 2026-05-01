# 🚀 Multi-Tier Web Application on AWS (kOps + Kubernetes)

<p align="center">
  <img src="https://img.shields.io/badge/Kubernetes-kOps-blue?logo=kubernetes">
  <img src="https://img.shields.io/badge/Cloud-AWS-orange?logo=amazonaws">
  <img src="https://img.shields.io/badge/Java-Tomcat-red?logo=apachetomcat">
  <img src="https://img.shields.io/badge/Database-MySQL-blue?logo=mysql">
  <img src="https://img.shields.io/badge/Cache-Memcached-green">
  <img src="https://img.shields.io/badge/Queue-RabbitMQ-orange?logo=rabbitmq">
  <img src="https://img.shields.io/badge/Ingress-NGINX-green?logo=nginx">
</p>

---

## 🧠 Overview

This project showcases a **production-style multi-tier architecture** deployed on a **Kubernetes cluster managed by kOps on AWS**.

It simulates a real-world system with:
- Stateful services (MySQL with persistent storage)
- Distributed caching (Memcached)
- Asynchronous communication (RabbitMQ)
- Scalable application layer (Tomcat)

> 🎯 Goal: Demonstrate real DevOps & Cloud engineering skills — not just theory.

---

## 🏗 Architecture

<p align="center">
  <img src="./architecture.png" width="80%">
</p>

### 🔍 Architecture Highlights
- Multi-tier separation (Web / App / Data)
- Persistent storage using **AWS EBS**
- Load balancing via **AWS ELB + NGINX Ingress**
- Decoupled services using **RabbitMQ**

---

## 🛠 Tech Stack

| Layer        | Technology |
|-------------|----------|
| Orchestration | Kubernetes (kOps) |
| Cloud         | AWS (EC2, EBS, ELB, S3) |
| Application   | Java (Tomcat) |
| Database      | MySQL (Persistent Volume - EBS) |
| Cache         | Memcached |
| Messaging     | RabbitMQ |
| Ingress       | NGINX Ingress Controller |

---

## ✨ Key Features

- ✅ **Highly Available Cluster** (multi-AZ setup)
- ✅ **Persistent Storage** using AWS EBS + PVC
- ✅ **Secrets Management** via Kubernetes Secrets
- ✅ **Scalable Architecture** (stateless + stateful separation)
- ✅ **Production-like Deployment Strategy**

---

## ⚙️ Setup & Deployment

### 🔹 1. Environment Preparation

```bash
sudo apt update
sudo snap install aws-cli --classic
aws configure
ssh-keygen -t ed25519
```

### 🔹 2. Install kubectl

```bash 
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

### 🔹 3. Create Kubernetes Cluster (kOps)

```bash
kops create cluster \
  --name=salma-app.site \
  --state=s3://kopsstate153 \
  --zones=us-east-1a,us-east-1b \
  --node-count=2 \
  --node-size=t3.small \
  --control-plane-size=t3.medium \
  --dns-zone=salma-app.site \
  --ssh-public-key ~/.ssh/id_ed25519.pub
kops update cluster \
  --name=salma-app.site \
  --state=s3://kopsstate153 \
  --yes --admin
```

### 🔹 4. Deploy Application

```bash
# Deploy Ingress Controller
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.9.5/deploy/static/provider/aws/deploy.yaml

# Clone project
git clone https://github.com/SalmaEasa/vprokube.git

# Deploy workloads
kubectl apply -f vprokube/kubedefs/
```

### 🚦 Cluster Management Commands

| Action   | Description        | Command |
|----------|-------------------|--------|
| Validate | Check cluster status | `kops validate cluster --state=s3://kopsstate153` |
| Delete   | Remove the cluster  | `kops delete cluster --name=salma-app.site --state=s3://kopsstate153 --yes` |

### 🔧 Challenges & Solutions
- 🧩 YAML Errors
Faced strict decoding issues due to incorrect indentation
✔ Fixed by restructuring metadata and labels
- 💸 Cost Management
AWS resources kept running unintentionally
✔ Solved by automating cleanup using kops delete cluster

### 📸 Demo / Screenshots (Optional but STRONGLY recommended)
Kubernetes Pods Running
Application UI
AWS Console (EC2 / ELB)

📌 What This Project Demonstrates
- Real-world Kubernetes deployment (not just Minikube)
- Cloud infrastructure provisioning (AWS + kOps)
- Handling stateful workloads in Kubernetes
- Understanding of distributed system components

### 👩‍💻 Author

Salma Easa
📍 Aspiring DevOps / Cloud Engineer
