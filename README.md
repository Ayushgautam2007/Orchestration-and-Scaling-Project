# Orchestration and Scaling Project

## Project Overview

This project demonstrates the implementation of a complete CI/CD pipeline for a MERN-based microservices application using AWS Cloud services, Docker, Jenkins, Kubernetes (Amazon EKS), Helm, CloudWatch, and SNS.

The objective of this project was to containerize the application, automate the build and deployment process, deploy the application on Amazon EKS, and configure monitoring, logging, and alerting mechanisms.

---

## Technologies Used

* ReactJS
* Node.js
* Express.js
* MongoDB
* Docker
* Amazon ECR
* Jenkins
* Amazon EKS
* Kubernetes
* Helm
* Amazon CloudWatch
* Amazon SNS
* Git & GitHub

---

## Project Architecture

Developer → GitHub → Jenkins → Docker Build → Amazon ECR → Amazon EKS → Helm Deployment → CloudWatch Monitoring → SNS Notifications

---

## Application Components

### Frontend

* ReactJS based user interface
* Containerized using Docker
* Exposed through Kubernetes LoadBalancer Service

### Hello Service

* Node.js microservice
* Runs on Port 3001
* Provides greeting APIs

### Profile Service

* Node.js microservice
* Runs on Port 3002
* Connected with MongoDB
* Provides user profile APIs

---

## Project Structure

```text
Orchestration-and-Scaling-Project/
│
├── backend/
│   ├── helloservice/
│   └── profileservice/
│
├── frontend/
│
├── helm/
│   └── mern-stack/
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
│
├── Jenkinsfile
│
├── screenshots/
│
└── README.md
```

---

## Implementation Steps

### Step 1: Version Control with Git

* Forked and cloned the project repository.
* Managed source code using Git and GitHub.
* Maintained version control throughout the project.

### Step 2: Containerization

* Created Dockerfiles for:

  * Frontend
  * Hello Service
  * Profile Service
* Built Docker images locally.
* Verified application functionality inside containers.

### Step 3: Amazon ECR

* Created separate ECR repositories.
* Tagged Docker images appropriately.
* Pushed images to Amazon ECR.

Repositories used:

* frontend
* helloservice
* profileservice

### Step 4: Continuous Integration using Jenkins

* Installed Jenkins on AWS EC2.
* Configured required plugins and credentials.
* Created Jenkins Pipeline.
* Automated:

  * Source Code Checkout
  * Docker Build
  * ECR Push
* Enabled automatic builds through GitHub integration.

### Step 5: Kubernetes Deployment using Amazon EKS

* Created Amazon EKS Cluster.
* Configured worker nodes.
* Deployed application using Helm charts.
* Created:

  * Deployments
  * Services
  * ReplicaSets
  * Load Balancer

Helm deployment command:

```bash
helm install mern-stack .
```

### Step 6: Monitoring and Logging

Configured Amazon CloudWatch Container Insights.

Implemented:

#### Monitoring

* Cluster Metrics
* Node Metrics
* Pod Metrics

#### Logging

Centralized logs using CloudWatch Logs.

Log Groups:

```text
/aws/containerinsights/Orchestration-cluster/application
/aws/containerinsights/Orchestration-cluster/dataplane
/aws/containerinsights/Orchestration-cluster/host
/aws/containerinsights/Orchestration-cluster/performance
```

### Step 7: CloudWatch Alarm

Created CloudWatch Alarm for monitoring EKS node utilization.

Example Alarm:

* EKS-Node-CPU-High

Purpose:

* Monitor infrastructure health
* Generate notifications when thresholds are exceeded

### Step 8: ChatOps / Notifications

Configured Amazon SNS Topic for notifications.

Features:

* Email subscription
* Alert delivery
* Monitoring notifications

---

## Validation Performed

### Kubernetes Resources

```bash
kubectl get pods
kubectl get svc
helm list
```

### Application Verification

Hello Service:

```bash
curl http://localhost:3001
```

Profile Service Health Check:

```bash
curl http://localhost:3002/health
```

CloudWatch Logs:

```bash
aws logs tail "/aws/containerinsights/Orchestration-cluster/application"
```

---

## Results

Successfully implemented:

* Microservices Architecture
* Docker Containerization
* Amazon ECR Integration
* Jenkins CI Pipeline
* Amazon EKS Deployment
* Helm Packaging
* CloudWatch Monitoring
* Centralized Logging
* SNS Notifications

---

## Cost Optimization Note

After successful implementation, testing, validation, and documentation of the project, all AWS resources were terminated/deleted to avoid unnecessary cloud charges.

The following resources were cleaned up:

* Amazon EKS Cluster
* Worker Nodes
* EC2 Instances
* Load Balancers
* CloudWatch Resources (where applicable)
* Other temporary infrastructure resources

This was done as the project was completed and keeping the resources running would incur additional AWS costs.

---

