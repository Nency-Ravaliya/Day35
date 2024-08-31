# Deploying a Multi-Tier Application Using Helm on Kubernetes and AWS Free Tier Services

This project demonstrates the deployment of a multi-tier application using Helm on Minikube, integrated with AWS free-tier services such as S3 for storage and RDS (MySQL) for the database. The focus is on Helm chart management, secrets, RBAC, and cloud resource management.

## Prerequisites
- Minikube installed and running.
- Helm installed on the local machine.
- AWS account with S3 and RDS services.
- kubectl configured to interact with your Kubernetes cluster.

## Project Objectives
- Deploy a multi-tier application using Helm on Minikube.
- Integrate AWS free-tier services (S3 and RDS).
- Manage Helm charts, including versioning, packaging, and rollbacks.
- Implement Helm secrets management and RBAC.
- Handle dependencies between different components of the application.

## Project Deliverables

### 1. Setup Helm and Minikube
- Ensure Minikube is running.

- Install and configure Helm on the local machine:
  ```bash
  curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
  helm repo add stable https://charts.helm.sh/stable
  helm repo update
  ```
  
### 2. AWS Services Setup
S3 Bucket: Create an S3 bucket for storing application assets (e.g., static files for the frontend).
RDS Instance: Set up an Amazon RDS MySQL instance in the free tier.

### 3. Create Helm Charts
Frontend Chart: Create a Helm chart for a frontend service (e.g., NGINX) that pulls static files from the S3 bucket:

`helm create frontend`

Backend Chart: Create a Helm chart for a backend service (e.g., a Python Flask API) that connects to the RDS MySQL database:
`helm create backend`

### 4. Package Helm Charts
Package each Helm chart into a .tgz file:

```
helm package frontend
helm package backend
```

Ensure charts are properly versioned.

### 5. Deploy Multi-Tier Application Using Helm
Deploy the database chart (connected to the RDS instance).

Deploy the backend chart with a dependency on the database chart:

`helm install backend backend-1.1.0.tgz`

Deploy the frontend chart with a dependency on the backend service, ensuring it pulls assets from the S3 bucket:

`helm install frontend frontend-1.1.0.tgz`

###  Implement RBAC
Define RBAC roles and role bindings to manage permissions for Helm deployments:

```
kubectl apply -f helm-service-account.yaml
kubectl apply -f helm-Role.yaml
kubectl apply -f helm-rolebinding.yaml
```

Ensure that only authorized users can deploy or modify the Helm releases:

`kubectl auth can-i get pods --as system:serviceaccount:default:helm-service-account`

Versioning and Rollback
Update the version of one of the Helm charts (e.g., update the frontend service):

```
image:
  repository: <image>
  pullPolicy: IfNotPresent
  tag: <tag>  # changed the image version from 3 to 4
```
Upgrade the cluster using the following command:

```
helm package frontend
helm upgrade frontend frontend-1.1.1.tgz
```

Verify the new version of the frontend service:

```
helm history frontend
```

Perform a rollback if necessary and validate the application functionality:

```
helm rollback frontend 1
helm history frontend
```

### Validate Deployment

Ensure the frontend service is serving files from the S3 bucket:

```
kubectl port-forward service/frontend 8085:80
```

Validate that the backend service is successfully communicating with the RDS MySQL database.

Test the overall functionality of the deployed application:

```
kubectl get pods
kubectl get services
kubectl get deployments
```

### Cleanup
Delete all Helm releases and Kubernetes resources created during the project:

```
helm uninstall frontend
helm uninstall backend
```

Terminate the RDS instance and delete the S3 bucket.

Stop Minikube if no longer needed.


# Output

![image](https://github.com/user-attachments/assets/de515cd0-cfde-47fe-a52b-f45d42f706c8)
![image](https://github.com/user-attachments/assets/f35c1aaf-5891-427b-a04b-c4afe308f99d)
![image](https://github.com/user-attachments/assets/4e828bb6-5f07-4738-9109-d4ed9152bbb6)
![image](https://github.com/user-attachments/assets/08a7a48b-b8d4-473b-8263-0baa62cedd81)
![image](https://github.com/user-attachments/assets/11b44022-1103-4b56-9408-873364da4f50)
![image](https://github.com/user-attachments/assets/66eeaf7e-5571-48e4-bc46-3602ec0b1280)
![image](https://github.com/user-attachments/assets/b1f01e71-3ac0-4ce4-af0c-89d4717738e7)
![image](https://github.com/user-attachments/assets/291eb05d-b8bd-4f12-8c4b-3c260665b853)
![image](https://github.com/user-attachments/assets/bb44481f-0d67-4b4c-949c-a686e00cd420)
![image](https://github.com/user-attachments/assets/b32db9ea-6aa9-4e37-b519-8901c03914dc)
![image](https://github.com/user-attachments/assets/99999585-6a20-467c-addb-f2f16af76705)
![image](https://github.com/user-attachments/assets/4a540d25-60db-42c1-b2fc-5917fca705bd)
![image](https://github.com/user-attachments/assets/3a60effb-df06-4f43-8035-a01777db6bd1)
![image](https://github.com/user-attachments/assets/cda9a965-06d1-4749-a59c-bda05bacd3a0)
![image](https://github.com/user-attachments/assets/4adb2107-a477-4ad4-86c9-ffa1043bac8b)
![image](https://github.com/user-attachments/assets/8130e8c4-4f16-465f-8137-5d917031b8d4)
![image](https://github.com/user-attachments/assets/4ec93373-e144-4a0d-87eb-fcc3283aba3a)
![image](https://github.com/user-attachments/assets/accef7fb-ed6a-4afb-860f-ede0fd38bf31)
![image](https://github.com/user-attachments/assets/0af143cb-949c-4e54-9bba-fab8fb3375e7)
![image](https://github.com/user-attachments/assets/3fca540c-5516-4bf0-929e-eec8b213ef68)
![image](https://github.com/user-attachments/assets/6fb4282c-35dc-4029-b767-360a11b40202)
![image](https://github.com/user-attachments/assets/f1115fe1-a9f4-4306-9c5c-5a81215eabc1)




















