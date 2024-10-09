
# WordPress & MySQL Kubernetes Deployment

This repository contains the Kubernetes deployment configuration for running WordPress and MySQL in a Kubernetes cluster, enhanced for reliability and scalability.

## Overview

This deployment includes:
- **WordPress**: Deployed as a scalable application using Kubernetes `Deployment` with 2 replicas.
- **MySQL**: Managed using a `StatefulSet` to maintain data persistence across pod restarts.
- **NGINX Ingress**: Used to manage external access to the WordPress application.
- **Secrets and PVCs**: For secure password management and persistent data storage.

## Prerequisites

- A running Kubernetes cluster (Minikube or AWS EKS).
- kubectl installed and configured.
- Helm installed (for NGINX Ingress controller).
- AWS ECR repository for Docker images.

## Setup

1. **Clone the repository**:
   ```bash
   git clone <repo-link>
   cd k8s-project
   ```

2. **Create Kubernetes Secrets**:
   Secure the MySQL credentials using Kubernetes secrets.
   ```bash
   kubectl apply -f mysql-secret.yaml
   ```

3. **Deploy MySQL**:
   Deploy the StatefulSet and Service for MySQL.
   ```bash
   kubectl apply -f mysql-statefulset.yaml
   kubectl apply -f mysql-service.yaml
   ```

4. **Deploy WordPress**:
   Deploy WordPress using the provided Deployment and Service configuration.
   ```bash
   kubectl apply -f wordpress-deployment.yaml
   kubectl apply -f wordpress-service.yaml
   ```

5. **Install NGINX Ingress**:
   Install the NGINX Ingress Controller using Helm:
   ```bash
   helm install ingress-nginx ingress-nginx/ingress-nginx --namespace ingress-nginx
   kubectl apply -f wordpress-ingress.yaml
   ```

6. **Monitor the Deployment**:
   Ensure the deployment is running correctly:
   ```bash
   kubectl get pods
   kubectl get svc
   ```

## Persistent Storage

Persistent Volumes and Persistent Volume Claims are set up to ensure data is preserved across pod restarts:
- MySQL data is stored in a persistent volume.
- WordPress media and uploads are stored in a separate volume.

## Grafana Monitoring (EKS Only)

To monitor the uptime and health of the WordPress containers, you can use Grafana:
- **Access Grafana**:
   ```bash
   kubectl get svc -n monitoring
   ```

## Troubleshooting

If the WordPress site isn't accessible, check:
- The status of the Ingress controller.
- The status of the MySQL StatefulSet and its associated PVCs.

## Contributions

Feel free to open issues or submit pull requests to improve this setup!
