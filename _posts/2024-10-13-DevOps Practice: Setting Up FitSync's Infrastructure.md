---
title: "DevOps Practice: My Journey in Setting Up FitSync's Infrastructure"
categories:
  - projects
tags:
  - DevOps
  - Cloud Infrastructure
  - Azure
  - Kubernetes
sidebar:
  nav: "sidebar-category"
toc: true
toc_sticky: true
toc_label: "My Infrastructure Setup Process"
header:
  teaser: "/assets/posting/cloud-computing.png"
---

## 📌 Project Overview

To deploy the FitSync project, I took on the challenge of setting up a robust, scalable cloud infrastructure. This post details my personal journey through each step of the process, highlighting the decisions I made and the actions I took to bring our infrastructure to life.

## 🚀 Step-by-Step Implementation

### 1. Setting Up Infrastructure Using Terraform

I began by creating the foundation of our infrastructure using Terraform:

1. **Azure Login**: I started by logging into Azure using the CLI:

   ```bash
   az login
   ```

2. **Creating Azure Container Registry**: I set up our container registry to store Docker images:

   ```bash
   az acr create --resource-group fitsync --name fitsync --sku Basic
   ```

3. **Project Directory Setup**: I created and navigated to a new project directory:

   ```bash
   mkdir FitSync-infra && cd FitSync-infra
   ```

4. **Defining Infrastructure**: I wrote the `main.tf` file to define our required resources. Here's a snippet of what I created:

   ```hcl
   resource "azurerm_kubernetes_cluster" "aks" {
     name                = "fitsync-aks"
     location            = azurerm_resource_group.rg.location
     resource_group_name = azurerm_resource_group.rg.name
     dns_prefix          = "fitsync"

     default_node_pool {
       name       = "default"
       node_count = 3
       vm_size    = "Standard_D2_v2"
     }

     identity {
       type = "SystemAssigned"
     }
   }
   ```

### 2. Initializing, Planning, and Applying Infrastructure

With the Terraform configuration in place, I proceeded to create the Azure resources:

1. **Terraform Initialization**: I ran the following command to download and install necessary Terraform plugins:

   ```bash
   terraform init
   ```

2. **Planning Infrastructure Changes**: I generated an execution plan to preview the changes:

   ```bash
   terraform plan
   ```

3. **Applying Infrastructure Changes**: I applied the configuration to create the resources in Azure:

   ```bash
   terraform apply
   ```

### 3. Deploying Docker Containers to ACR

Next, I focused on containerizing our microservices:

1. **ACR Login**: I logged into our Azure Container Registry:

   ```bash
   az acr login --name fitsyncacr
   ```

2. **Building and Tagging Docker Images**: I built and tagged Docker images for each microservice. For example, for the user service:

   ```bash
   docker build -t fitsync-user-service .
   docker tag fitsync-user-service fitsyncacr.azurecr.io/fitsync-user-service:latest
   ```

3. **Pushing Docker Images**: I pushed the images to our ACR:

   ```bash
   docker push fitsyncacr.azurecr.io/fitsync-user-service:latest
   ```

   I repeated these steps for each of our microservices.

### 4. Deploying Infrastructure Using Helm

To streamline our Kubernetes deployments, I utilized Helm:

1. **Helm Initialization**: I initialized Helm in our cluster:

   ```bash
   helm init
   ```

2. **Deploying Microservices**: I created Helm charts for our microservices and deployed them to the AKS cluster:

   ```bash
   helm install ./chart --name fitsync-app
   ```

### 5. Setting Up Monitoring with Prometheus and Grafana

To ensure we could effectively monitor our infrastructure, I set up Prometheus and Grafana:

1. **Deploying Prometheus**: I used Helm to deploy Prometheus:

   ```bash
   helm install stable/prometheus --name prometheus
   ```

2. **Deploying Grafana**: Similarly, I deployed Grafana:

   ```bash
   helm install stable/grafana --name grafana
   ```

3. **Configuring Dashboards**: I accessed the Grafana dashboard and created visualizations for our key metrics.

### 6. Configuring Auto Scaling and Load Testing

To ensure our system could handle varying loads, I set up auto-scaling and conducted load tests:

1. **Auto Scaling Configuration**: I modified the `values.yaml` file to activate the HorizontalPodAutoscaler:

   ```yaml
   autoscaling:
     enabled: true
     minReplicas: 1
     maxReplicas: 10
     targetCPUUtilizationPercentage: 50
     targetMemoryUtilizationPercentage: 50
   ```

   I applied these changes using Helm:

   ```bash
   helm upgrade fitsync-backend ./fitsync-chart
   ```

2. **Load Testing**: I used Apache Bench to simulate load:

   ```bash
   ab -n 10000 -c 100 http://<YOUR-SERVICE-EXTERNAL-IP>/user-service/health
   ```

### 7. Monitoring Scaling and Results

Finally, I closely monitored the infrastructure's performance during and after the load test:

1. **Grafana Monitoring**: I observed metrics such as CPU usage, memory usage, request rates, and response times in the Grafana dashboards I had set up.

2. **HPA Status Check**: I monitored the HorizontalPodAutoscaler status:

   ```bash
   kubectl get hpa -w
   ```

3. **Pod Scaling Observation**: I watched the number of pods in real-time:

   ```bash
   kubectl get pods -w
   ```

## 🎉 Conclusion

Through this process, I single-handedly set up a robust, scalable, and monitored infrastructure for FitSync. Each step presented its own challenges, but overcoming them has greatly enhanced my skills as a DevOps engineer. 

The infrastructure I've put in place will serve as a solid foundation for FitSync's growth, capable of handling increasing loads while maintaining performance and reliability. As we move forward, I'm excited to continue refining and optimizing this setup to meet the evolving needs of our application and users.