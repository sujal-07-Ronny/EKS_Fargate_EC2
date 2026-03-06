# AWS EKS Hybrid Deployment (EC2 + Fargate)

This project demonstrates a **hybrid Kubernetes architecture on Amazon EKS** where workloads run on both **EC2 worker nodes** and **AWS Fargate serverless compute**.

The goal of this experiment was to understand how Kubernetes schedules pods across different compute models and how AWS EKS enables flexible infrastructure management.

---

## 🚀 Project Overview

In Amazon EKS, workloads can run in two ways:

**1️⃣ EC2 Worker Nodes**

* You manage the underlying instances
* Suitable for long-running applications
* Provides full control over infrastructure

**2️⃣ AWS Fargate**

* Serverless compute for containers
* No server management required
* AWS provisions CPU and memory automatically

This project demonstrates how **both models can coexist inside the same Kubernetes cluster**.

---

## 🧠 Key Learning Objectives

* Deploy applications on **EC2 node groups**
* Deploy workloads using **AWS Fargate profiles**
* Understand **pod scheduling behavior**
* Troubleshoot **Pending pods in Kubernetes**
* Observe **serverless pod lifecycle in EKS**

---

## ⚙️ Technologies Used

* **Amazon EKS**
* **AWS Fargate**
* **Kubernetes**
* **eksctl**
* **kubectl**
* **Docker (NGINX container)**
* **AWS CLI**

---

## 📁 Project Structure

```
EKS_Fargate_EC2
│
├── ec2-app.yaml
├── fargate-app.yaml
├── README.md
└── screenshots
    ├── fargate-running.png
    └── pod-lifecycle.png
```

---

## 🔧 Deployment Steps

### 1️⃣ Create EKS Cluster

```
eksctl create cluster --name eks-fargate-demo --region ap-south-1
```

### 2️⃣ Create Fargate Profile

```
eksctl create fargateprofile --cluster eks-fargate-demo --region ap-south-1 --name fargate-profile --namespace fargate-app
```

### 3️⃣ Deploy Application on EC2 Node Group

```
kubectl apply -f ec2-app.yaml
```

### 4️⃣ Deploy Application on Fargate

```
kubectl apply -f fargate-app.yaml
```

### 5️⃣ Verify Pods

```
kubectl get pods -A -o wide
```

Pod lifecycle observed:

```
Pending → ContainerCreating → Running
```

---

## 🔍 Observations

During deployment, pods initially remained in **Pending state** due to node capacity limitations on small EC2 instances.

This highlighted an important Kubernetes behavior:

> Pods may remain pending if node resources or pod density limits are reached.

After configuring a **Fargate profile**, Kubernetes successfully scheduled the workload on **AWS-managed serverless infrastructure**.

---

## 🏗 Real-World Production Architecture

Organizations often run **hybrid EKS environments**:

**EC2 Nodes**

* High traffic services
* Stateful workloads
* GPU workloads
* Custom networking requirements

**Fargate**

* Microservices
* Event-driven applications
* Batch jobs
* CI/CD workloads
* Short-lived containers

This hybrid model provides **flexibility, scalability, and operational efficiency**.

---

## 📸 Screenshots

Example outputs showing:

* Pod lifecycle
* Serverless pod scheduling
* Running workloads on Fargate

*(See screenshots folder)*

---

## 📚 Key Takeaways

* Amazon EKS supports both **managed EC2 infrastructure and serverless Fargate workloads**
* Fargate eliminates the need to manage Kubernetes worker nodes
* Understanding Kubernetes scheduling behavior is critical for production environments
* Hybrid compute models provide the best balance of **control, scalability, and cost efficiency**

---

## 👨‍💻 Author

**Sujal Shah**

DevOps | Cloud | Kubernetes Enthusiast
