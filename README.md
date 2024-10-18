# Deployment and Setup Guide

This guide provides a step-by-step procedure to build, tag, push Docker images to Docker Hub, deploy microservices to Kubernetes (Minikube), and run the application using Streamlit.

## 1. Building and Tagging Docker Images

### Step 1: Build Docker Images

Run the command `docker-compose up --build` to build the Docker images. After the build is complete, three Docker images will be created. Check Docker Desktop to confirm that the images have been successfully created.

### Step 2: Tag Docker Images

Tag the Docker images using the Docker CLI command as per the official Docker documentation. An example of tagging an image is `docker image tag <source_image> <your_dockerhub_username>/<image_name>:<tag>`. Do this for all three images and push them to your Docker Hub using `docker push <your_dockerhub_username>/<image_name>:<tag>`.

### Step 3: Update YAML Files

In all the YAML files, replace all occurrences of the image `vibhagangolli/order_processing_service:latest` with your Docker Hub image names.

### Step 4: Verify Docker Hub

Log into your Docker Hub account and confirm that all images have been successfully pushed.

### Step 5: Stop Docker

After verifying, stop the Docker containers and services by running `docker-compose down`.

## 2. Setting Up Kubernetes on Minikube

### Step 1: Start Minikube

Start Minikube by running `minikube start`.

### Step 2: Apply YAML Configurations

Apply the microservice YAML configurations to the Minikube cluster using the following commands:
- `kubectl apply -f product-management-service.yaml`
- `kubectl apply -f user-auth-service.yaml`
- `kubectl apply -f product-management-deployment.yaml`
- `kubectl apply -f user-auth-deployment.yaml`
- `kubectl apply -f order-processing-deployment.yaml`
- `kubectl apply -f order-processing-service.yaml`
- `kubectl apply -f microservices-network-networkpolicy.yaml`

## 3. Configure Base URLs in `ecom_st.py`

Open the `ecom_st.py` file and modify the first three lines to set the URLs of your services as follows:
- `AUTH_BASE_URL = ""`
- `PRODUCT_BASE_URL = ""`
- `ORDER_BASE_URL = ""`

To get the respective URLs, run the following commands in three separate terminals:
- For `ORDER_BASE_URL`, run `minikube service order-processing --url`.
- For `PRODUCT_BASE_URL`, run `minikube service product-management --url`.
- For `AUTH_BASE_URL`, run `minikube service user-auth --url`.

Copy the URLs outputted from these commands and replace them in the `ecom_st.py` file.

## 4. Verifying Microservices Deployment

In a new terminal, verify that all services, deployments, and pods are running by executing the following commands:
- `kubectl get svc`
- `kubectl get deployment`
- `kubectl get pod`

This should list the `order-processing`, `product-management`, and `user-auth` microservices.

## 5. Running the Application with Streamlit

After everything is set up, run the application by executing `streamlit run ecom_st.py`.

## 6. Cleanup

Once you're done, delete the Kubernetes services and deployments with the following commands:
- `kubectl delete svc order-processing product-management user-auth`
- `kubectl delete deployment order-processing product-management user-auth`

This guide should help you deploy the microservices architecture on Minikube, tag and push Docker images, and run the Streamlit e-commerce app.

For any questions, feel free to reach out at vibgang@gmail.com. Happy Coding! 😊

---
