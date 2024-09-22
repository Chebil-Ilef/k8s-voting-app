# Voting App Kubernetes

This repository contains Kubernetes manifests to deploy a voting application with its supporting services using Minikube and Kubernetes.

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Deploy the Application](#deploy-the-application)
- [Accessing Services](#accessing-services)
- [Scaling the Application](#scaling-the-application)
- [Application Architecture](#application-architecture)

## Overview

The voting app consists of the following components:
- **Voting app**: A frontend service where users vote.
- **Redis**: A queue service to store votes.
- **Worker**: A backend worker to process votes.
- **PostgreSQL**: A database to store processed vote results.
- **Result app**: A frontend service to display the voting results.

## Prerequisites

Before you begin, make sure you have the following installed:
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

Start Minikube with sufficient resources (optional, but recommended):

```bash
minikube start --memory=4096 --cpus=2
```

## Deploy the Application

### 1. Deploy the Voting App

```bash
kubectl create -f voting-app-deploy.yaml
kubectl create -f voting-app-service.yaml
kubectl get deployments
```

### 2. Deploy Redis, PostgreSQL, Worker, and Result App

Repeat the following commands for Redis, PostgreSQL, worker, and result app:

```bash
# Redis
kubectl create -f redis-deploy.yaml
kubectl create -f redis-service.yaml

# PostgreSQL
kubectl create -f postgres-deploy.yaml
kubectl create -f postgres-service.yaml

# Worker
kubectl create -f worker-app-deploy.yaml

# Result app
kubectl create -f result-app-deploy.yaml
kubectl create -f result-app-service.yaml
```

You can verify the deployments with:

```bash
kubectl get deployments
```

## Accessing Services

To access the services via Minikube, use the following commands:

```bash
minikube service voting-service --url
minikube service result-service --url
```

This will provide you with the URLs to access the Voting and Result apps.

## Scaling the Application

To scale the voting app deployment, use the following command to increase the replicas:

```bash
kubectl scale deployment voting-app-deploy --replicas=3
```

Verify the scaling with:

```bash
kubectl get deployments voting-app-deploy
```

## Application Architecture

Below is the architecture diagram illustrating the interaction between the different components of the system:

![Voting App Architecture](./architecture/architecture%20voting%20app.PNG)

- **Voting app**: Handles user votes.
- **Redis**: Stores votes temporarily.
- **Worker**: Processes votes and stores them in PostgreSQL.
- **PostgreSQL**: Stores the final results.
- **Result app**: Displays the results.
