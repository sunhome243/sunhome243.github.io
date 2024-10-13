---
title: "DevOps Practice: Overcoming Challenges in FitSync Infrastructure"
categories:
  - projects
tags:
  - DevOps
  - Troubleshooting
  - Cloud Infrastructure
  - Azure
sidebar:
  nav: "sidebar-category"
toc: true
toc_sticky: true
toc_label: "Error Notes and Solutions"
header:
  teaser: "/assets/posting/errorteaser.jpg"
---

## 📌 Overview

During the FitSync project, I encountered and overcame numerous challenges during the setup of our cloud infrastructure. This document details the errors I faced and the solutions I implemented.

## 🛠️ Errors and Solutions

### 1. Terraform Configuration Issues

#### Missing `subscription_id` Error

**Error**: 
When running `terraform plan`, I encountered an error indicating that `subscription_id` is a required provider property.

**Solution**:
I resolved this by updating the `main.tf` file to include the `subscription_id` parameter:

```hcl
provider "azurerm" {
  features {}
  subscription_id = "<Subscription id>"
}
```

Alternatively, I set the subscription ID as an environment variable:

```bash
export ARM_SUBSCRIPTION_ID=$(az account show --query id -o tsv)
```

### 2. Azure Kubernetes Service (AKS) Configuration

#### VM Size Not Allowed for AKS

**Error**: 
The VM size `Standard_B2s` was not allowed for the selected Azure region (`eastus`) when creating an AKS cluster.

**Solution**:
I resolved this by first checking the allowed VM sizes in the selected region:

```bash
az vm list-sizes --location eastus
```

Then, I updated the `main.tf` configuration with an allowed VM size:

```hcl
default_node_pool {
  name       = "default"
  node_count = 1
  vm_size    = "Standard_B2pls_v2"
}
```

### 3. Kubernetes Autoscaling Issues

#### HorizontalPodAutoscaler (HPA) Not Scaling as Expected

**Error**: 
The HorizontalPodAutoscaler (HPA) was not scaling the pods as expected during load testing.

**Solution**:
I implemented a two-part solution:

1. Verified that resource requests and limits were set properly in the `values.yaml` file:

```yaml
resources:
  limits:
    cpu: 500m
    memory: 512Mi
  requests:
    cpu: 250m
    memory: 256Mi
```

2. Ensured that the metrics server was running in the Kubernetes cluster:

```bash
kubectl get deployment metrics-server -n kube-system
```

### 4. Monitoring Setup Challenges

#### Grafana Access Issues

**Error**: 
Unable to access Grafana dashboard externally after deploying it with Helm.

**Solution**:
I resolved this by patching the Grafana service to make it accessible externally:

```bash
kubectl patch svc grafana -p '{"spec": {"type": "LoadBalancer"}}'
```

Then, I retrieved the external IP of the Grafana service:

```bash
kubectl get svc grafana
```

#### Prometheus Data Source Not Working in Grafana

**Error**: 
Unable to configure Prometheus as a data source in Grafana.

**Solution**:
I fixed this by first retrieving the Prometheus service URL:

```bash
kubectl get services | grep prometheus
```

Then, I configured the data source in Grafana using the Prometheus service URL (e.g., `http://prometheus-server.default.svc.cluster.local`).

### 5. Database Connection Issues

#### PostgreSQL Connection Errors

I encountered and resolved several PostgreSQL connection issues:

1. **Invalid Username Format**:
   - Error: `sqlalchemy.exc.OperationalError: (psycopg2.OperationalError) Invalid Username specified.`
   - Solution: Updated the connection string to use the correct format: `fitsyncadmin@fitsyncpsqlserver`

2. **SSL Connection Required**:
   - Error: `FATAL: SSL connection is required.`
   - Solution: Added `?sslmode=require` to the connection string

3. **Password Encoding Issue**:
   - Error: `configparser.InterpolationSyntaxError: '%' must be followed by '%' or '('`
   - Solution: Applied proper URL encoding to the password in the connection string

4. **Alembic Migration Fails**:
   - Error: `sqlalchemy.exc.ArgumentError: Could not parse SQLAlchemy URL`
   - Solution: Corrected the connection string format in the `alembic.ini` file

### 6. Kubernetes Ingress and Service Issues

#### 404 Not Found for Health Check

**Error**: 
`GET /workout-service/health HTTP/1.1 404`

**Solution**:
I updated the Ingress configuration to fix the path routing, specifically correcting the rewrite-target annotation:

```yaml
nginx.ingress.kubernetes.io/rewrite-target: /$2
```

#### Path Prefix Issues in Ingress

**Error**: 
`Warning: path /workout-service(/|$)(.*) cannot be used with pathType Prefix`

**Solution**:
I simplified the path to use just `/workout-service` without any regex-like patterns:

```yaml
- path: /workout-service
  pathType: Prefix
  backend:
    service:
      name: workout-service
      port:
        number: 80
```

#### Service Fails Liveness and Readiness Probes

**Error**: 
```
Warning  Unhealthy  Liveness probe failed: HTTP probe failed with statuscode: 404
Warning  Unhealthy  Readiness probe failed: HTTP probe failed with statuscode: 404
```

**Solution**:
I confirmed that the service was running correctly inside the pod using `kubectl exec` and querying the health endpoint. Then, I adjusted the Ingress path routing to ensure that the probes could reach the correct endpoint.

### 7. Docker and Azure Container Registry (ACR) Issues

#### Unauthorized Push to Azure Container Registry

**Error**: 
```
failed to authorize: failed to fetch oauth token: unexpected status from GET request to https://fitsyncacr.azurecr.io/oauth2/token?scope=repository%3Afitsync-workout-service%3Apull&scope=repository%3Afitsync-workout-service%3Apull%2Cpush&service=fitsyncacr.azurecr.io: 401 Unauthorized
```

**Solution**:
I resolved this by using the Azure CLI command to log into the registry:

```bash
az acr login --name fitsyncacr
```

## 💡 Lessons Learned

Through overcoming these challenges, I gained valuable insights:

1. **Importance of Proper Configuration**: Many issues stemmed from misconfigurations. I learned to double-check all configuration files and use validation tools where available.

2. **Understanding Cloud Provider Limitations**: Azure's VM size restrictions taught me to always verify resource availability in the chosen region before deployment.

3. **Monitoring is Crucial**: Setting up proper monitoring early helped in quickly identifying and resolving issues in production.

4. **Security Best Practices**: Dealing with connection issues reinforced the importance of following security best practices, especially regarding database connections and authentication.

5. **Kubernetes Complexity**: The various Kubernetes-related issues highlighted the complexity of managing a Kubernetes cluster and the importance of understanding its networking and scaling mechanisms.

## 🔮 Future Improvements

Based on these experiences, I plan to implement the following improvements:

1. **Automated Testing**: Develop a suite of automated tests to catch configuration and deployment issues early.

2. **Enhanced Documentation**: Create comprehensive documentation for our infrastructure setup and common troubleshooting steps.

3. **Streamlined CI/CD Pipeline**: Implement a more robust CI/CD pipeline that includes automated checks for common issues.

4. **Regular Audits**: Schedule regular infrastructure audits to proactively identify and address potential issues.

