# 🚀 CI/CD Pipeline Automation Using Jenkins, Docker, GitHub & Amazon EKS

This project demonstrates a fully automated CI/CD pipeline that builds, tests, packages, and deploys a containerized web application using **Jenkins**, **Docker Hub**, **GitHub Webhooks**, and **Amazon EKS**.  
Any update to the source code automatically triggers Jenkins to build a new Docker image, push it to Docker Hub, and update the running application on Kubernetes.

---

## 📌 Project Architecture

**GitHub → Jenkins → Docker Hub → Amazon EKS**

1. Developer pushes code to **GitHub**
2. GitHub Webhook notifies **Jenkins**
3. Jenkins automatically:
   - Clones repository  
   - Builds Docker image  
   - Tags & pushes image to **Docker Hub**  
   - Updates Kubernetes deployment on **Amazon EKS**
4. Updated version of the app is deployed with zero downtime.

<img width="1620" height="1764" alt="_- visual selection" src="https://github.com/user-attachments/assets/66513103-693d-461c-9f93-c00f4801c1a6" />

---

## 📂 Features

- End-to-end automated CI/CD pipeline  
- Docker image build & push through Jenkins  
- GitHub webhook triggers  
- Kubernetes rolling deployment  
- IAM-based secure AWS environment  
- EKS-managed scalable production environment  

---

## 🛠️ Technologies Used

- **AWS:** IAM, EKS, EC2, Node Groups  
- **Jenkins:** Pipeline, Docker Integration  
- **Docker:** Build & Push  
- **GitHub:** SCM + Webhooks  
- **Kubernetes:** Deployment, Services, Rolling Updates  
- **Linux Tools:** kubectl, AWS CLI

  
<img width="3060" height="1692" alt="_- visual selection (1)" src="https://github.com/user-attachments/assets/8557d903-dda5-46f2-a997-4443ba15ee25" />

---

## 📑 Project Setup Overview

### ✔️ 1. IAM Roles Creation

Two roles were created:

#### **a) EKS Cluster Role**
- AmazonEKSClusterPolicy  
- Name: `eks-cluster-role`

#### **b) EKS Node Group Role**
- AmazonEKSWorkerNodePolicy  
- AmazonEC2ContainerRegistryReadOnly  
- AmazonEKS_CNI_Policy  
- Name: `eks-node-role`

---

### ✔️ 2. EKS Cluster Setup

- Created **my-eks-cluster**
- Selected latest Kubernetes version  
- Chose the `eks-cluster-role`  
- Selected multiple subnets  
- Created **nodegroup-1** (t3.medium, desired=2 nodes)  

---

### ✔️ 3. EC2 Instance + Jenkins Setup

EC2 Details:
- Name: `CI_CD`
- OS: Amazon Linux
- Instance: t3.medium  
- Ports allowed: 8080 (Jenkins), 443, Kubernetes-required ports  

Installed:
- Java 17  
- Jenkins  
- Git  
- Docker  
- kubectl  
- AWS CLI  

Configured:
- Added Jenkins & ec2-user to Docker group  
- Copied kubeconfig to Jenkins directory  
- Installed Jenkins plugins (Docker Pipeline, kubectl CLI)  
- Added DockerHub credentials in Jenkins  

---

### ✔️ 4. Kubernetes Deployment

Applied backend, frontend, and database manifests:

```bash
kubectl apply -f db.yaml
kubectl apply -f be.yaml
kubectl apply -f fe.yaml
```
<img width="2241" height="1944" alt="_- visual selection (2)" src="https://github.com/user-attachments/assets/7dec416e-34ac-4487-aca5-b8c33809009f" />

---

### 🔔 GitHub Webhook Setup

To enable automatic builds in Jenkins whenever code is pushed to GitHub:

1. Go to **GitHub → Repository Settings → Webhooks**
2. Click **Add Webhook**
3. In the "Payload URL" field, enter:
http://<EC2-Public-IP>:8080/github-webhook/
4. Set **Content type** to:application/json

5. Choose the trigger:
- **Just the push event**

6. Click **Add Webhook**

Now every commit/push to GitHub will automatically trigger a Jenkins pipeline run.

---

## 🚀 Workflow (How the CI/CD Pipeline Works)

1. Developer makes changes to the web application and pushes the code to GitHub.  
2. GitHub Webhook immediately notifies Jenkins.  
3. Jenkins automatically triggers the CI/CD pipeline, which performs:  
   - Clone the latest code from GitHub  
   - Build a new Docker image  
   - Tag the image using the Jenkins build number  
   - Push the new image to Docker Hub  
4. Jenkins updates the Kubernetes deployment using:  
kubectl set image deployment/<deployment-name> <container-name>=<docker-image>:<tag>
5. Kubernetes performs a **rolling update**, replacing old pods with new pods.  
6. The latest version of the application goes live on Amazon EKS with **zero downtime**.

<img width="2860" height="2124" alt="_- visual selection (3)" src="https://github.com/user-attachments/assets/b663ae49-26fd-4117-9238-ced85a2de844" />

---

## 📸 Advantages of This CI/CD Setup

### ✔️ Fully Automated Deployment
No manual steps needed — the entire pipeline (build → push → deploy) runs automatically.

### ✔️ Zero Downtime Updates
Kubernetes rolling updates ensure that application updates happen smoothly without affecting users.

### ✔️ Faster Development Cycle
Every code change triggers a fresh build and deploy, improving delivery speed and feedback loops.

### ✔️ Scalable & Reliable Architecture
Amazon EKS manages worker nodes, scaling, networking, and load balancing.

### ✔️ Secure & Production Ready
IAM roles, DockerHub credentials, and Jenkins permissions provide a secure CI/CD environment.

### ✔️ Reusable Jenkins Pipeline
The pipeline script can easily be adapted for multiple microservices or projects.

---
