# Project Assignment #2: Cloud-Native App Best-Buy Application

This repository contains the source code, documentation, and deployment files for the Best Buy demo cloud-native application. The application follows microservices architecture and incorporates AI capabilities for product descriptions and image generation.

## Application Architecture

### Application Table

| Service              | Description                                                       | Notes                           |
| -------------------- | ----------------------------------------------------------------- | ------------------------------- |
| **Store-Front**      | Customer-facing app for browsing and placing orders              | -                               |
| **Store-Admin**      | Employee-facing app for managing products and viewing orders .    | -                               |
| **Order-Service**    | Handles order creation and sends data to the managed order queue. | Uses RabbitMQ for messaging.    |
| **Product-Service**  | Handles CRUD operations for product data.                         | -                               |
| **Makeline-Service** | Processes and completes orders by reading from the order queue.   | -                               |
| **AI-Service**       | Generates product descriptions and images.                        | Uses GPT-4 and DALL-E-3 models. |
| **Database**         | MongoDB for persisting order and product data.                    | -                               |


### Diagram

![Algonquin Pet Store On Steroids](https://github.com/user-attachments/assets/ca65182d-4497-4cb0-9125-ce32c821e4aa)

### Architecture Overview

**Store-Front**: React.js application deployed as a microservice to allow customers to browse products and place orders.

**Store-Admin**: Admin portal for employees to manage inventory and view customer orders.

**Order-Service**: Processes incoming orders and pushes them to Azure Service Bus.

**Product-Service**: Manages product data, interfacing with the database and the AI service.

**Makeline-Service**: Pulls order details from Azure Service Bus, processes them, and updates order status.

**AI-Service**: Integrates with OpenAI APIs to generate product descriptions and images dynamically.

**MongoDB**: Used as a database for storing product and order information.

### Deployment Instructions

#### Prerequisites

* Azure account for Azure Service Bus and Azure Kubernetes Service (AKS).

* Docker installed and configured.

* kubectl CLI installed and configured for your AKS cluster.

Access to OpenAI APIs for GPT-4 and DALL-E.

Steps

1. Clone the Repository:
```
 git clone https://karthikurala/best-buy 
 ```
```
 cd best-buy-app
```
2. Build and Push Docker Images:

   For each microservice, navigate to its directory and run:
```
docker build -t karthikurala/<service-name>:laatest .
docker push karthikurala/<service-name>:latest
```
3. Setup Azure Resources

Resource group name: assignment

Region: East US.

Click Review + Create and then Create.

4. Create AKS Clusters

- Select **agentpool**. This nodes will have the controlplane.
        - Set **node size** to `D2as_v4`.
        - **Scale method**: `Manual`
        - **Node count**: `1`
        - Click `update`
     - Click on **Add node pool**:
        - **Node pool name**: `finalnode`.
        - **Mode**: `User` 
        - Set **node size** to `D2as_v3`.
        - **Scale method**: `Manual`
        - **Node count**: `2`
        - Click `add`
   - Click **Review + Create**, and then **Create**. The deployment will take a few minutes.

**Deploy to Kubernetes** Apply the deployment YAML files:
```
kubectl apply -f Deployment Files/
 ```

4. Verify Deployment:
   Ensure all pods are running
```
kubectl get pods
```
5. Deploy Microservices

Apply the deployment YAML files for each microservice:

```
kubectl apply -f Deployment_Files/store-front.yaml -n best-buy-app
kubectl apply -f Deployment_Files/store-admin.yaml -n best-buy-app
kubectl apply -f Deployment_Files/order-service.yaml -n best-buy-app
kubectl apply -f Deployment_Files/product-service.yaml -n best-buy-app
kubectl apply -f Deployment_Files/makeline-service.yaml -n best-buy-app
kubectl apply -f Deployment_Files/ai-service.yaml -n best-buy-app

```
### Issues and Limitations
**Azure Service Bus Integration**
   - Replacing RabbitMQ required significant code for message handling
     
**Mismatch in Clusters"
   - kubectl context is pointing to a local Kind cluster, while the Azure portal is connected to your Azure AKS cluster, causing a mismatch in the clusters and resulting in pods visible in VS Code but not in the Azure portal.


## Repositories
This table lists the GitHub repositories for each of the microservices in the project:

| **Service**        | **Repository Link**                          |
|--------------------|----------------------------------------------|
| Store-Front        | https://github.com/KarthikUrala/store-frontl8 |
| Order-Service      | https://github.com/KarthikUrala/order-servicel8 |
| Product-Service    | https://github.com/KarthikUrala/product-servicel8 |
| Makeline-Service   | https://github.com/KarthikUrala/ai-servicel8 |
| Store-Admin        | https://hub.docker.com/repository/docker/karthikurala/store-adminl8/general |
| AI-Service         | https://github.com/KarthikUrala/ai-servicel8 |

## Docker Images
This table lists the docker images for each of the microservices in the project:

| **Service**        | **Docker Image Link**                          |
|--------------------|----------------------------------------------|
| Store-Front        | https://hub.docker.com/repository/docker/karthikurala/store-frontl8/general |
| Order-Service      | https://hub.docker.com/repository/docker/karthikurala/order-servicel8 |
| Product-Service    | https://hub.docker.com/repository/docker/karthikurala/product-servicel8/general |
| Makeline-Service   | https://hub.docker.com/repository/docker/karthikurala/makeline-servicel8/general |
| Store-Admin        | https://hub.docker.com/repository/docker/karthikurala/store-adminl8/general |
| AI-Service         | https://hub.docker.com/repository/docker/karthikurala/ai-servicel8/general |
