# End-to-End CI/CD Pipeline for A Go Web App On AWS EKS with GitHub Actions, Argo CD & Helm

This project demonstrates a fully automated, production-grade CI/CD pipeline for a Go (Golang) web application. It uses Docker, GitHub Actions for continuous integration, Argo CD for GitOps-based continuous delivery, and Helm to manage Kubernetes manifests on AWS EKS.

## 🧰 Tech Stack

| Category          | Tool/Service              |
|-------------------|---------------------------|
| Language          | Go (Golang)               |
| CI/CD             | GitHub Actions, Argo CD   |
| Containerization  | Docker                    |
| Kubernetes Config | Helm                      |
| Orchestration     | Kubernetes (AWS EKS)      |
| GitOps            | Argo CD                   |
| Cloud Provider    | AWS                       |     |
| VCS               | Git + GitHub              |


## 📌 Features

- 🛠️ Build and test a Go application locally before containerization
- 🐳 Multi-stage Docker build for efficient image size
- ✅ Automated CI with GitHub Actions to build & push Docker images
- ⛵ Helm charts for Kubernetes resource templating
- 🔁 GitOps with Argo CD for continuous delivery on AWS EKS
- ☁️ Optimized for cost-effective, scalable infrastructure using AWS Free Tier

```
go-web-app/
├── .github/
│   └── workflows/
│       └── ci-cd.yaml
├── Dockerfile
├── README.md
├── go.mod
├── go.sum
├── main.go
├── static/
│   ├── index.html
│   ├── css/
│   │   └── style.css
├── helm/
│   └── go-webapp/
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
│           ├── deployment.yaml
│           ├── service.yaml
│           └── ingress.yaml
├── k8s/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── ingress.yaml
```


## Running the server

To run the server, execute the following command:

```bash
go run main.go
```

The server will start on port 8080. You can access it by navigating to `http://localhost:8080/courses` in your web browser.

## Looks like this

![Website](static/images/golang-website.png) 

## Build & Run With Docker

```
docker build -t go-web-app .
```

![GoProj2](https://github.com/user-attachments/assets/8dd5376f-2591-4809-a983-ddf26a8622c0)

```
docker run -p 8000:8000 -it go-web-app
```

![goProj3](https://github.com/user-attachments/assets/7456e856-ff22-4c59-8700-2152a0a5388c)

 ## Tag Docker Image
Using the command below:

```
docker tag go-webapp <your-dockerhub-username>/go-webapp:latest
```

Then, using mine: 

```
docker tag go-web-app samley/go-web-app:latest
```

## Push to Docker Hub
Using the command below:

```
docker push <your-dockerhub-username>/go-webapp:latest
```

Then implementing it, :

```
docker push samley/go-web-app:latest
```

![GoProj4](https://github.com/user-attachments/assets/b38cf3b7-2abf-42a0-9c60-931dc78c135d)

## Kubernetes Deployment

Since we are using EKS, we need to authenticate our AWS account with our local machine. 
Ensure you have the AWS CLI installed on your local machine. 

On your terminal or command prompt, run the command below : 

```
aws configure
```

You will be prompted to enter:

**AWS Access Key ID:** Obtain this from your AWS Management Console (under IAM > Users > Your User > Security credentials).

**AWS Secret Access Key:** Also obtained from IAM as above.

**Default region name:** Specify the AWS region you want to work in (e.g., us-east-1).

Next, install EKS 

```
eksctl create cluster --name demo-cluster
```

## Deployment Steps

```
kubectl apply -f k8s/manifests/deployment.yaml
kubectl apply -f k8s/manifests/service.yaml
kubectl apply -f k8s/manifests/ingress.yaml
```

![GoProj5](https://github.com/user-attachments/assets/446247b3-93c1-4c81-8f4a-acadf2cad8f8)

![GoProj6](https://github.com/user-attachments/assets/7006b059-2f05-45cb-bc15-d02548cf1ee1)

To ensure that the service.yaml is working fine, let's expose the service in NodePort mode.
We do that by editing the Service.yaml type to Nodeport using the command below :

```
kubectl edit svc go-web-app
```

To get the Service.yaml port number, we use the command :

```
kubectl get svc
```

![GoProj7](https://github.com/user-attachments/assets/747602ea-94df-44a2-be4a-5f18803b9cdd)

To get the nodes IP address : 

```
kubectl get nodes -o wide
```

![GoProj8](https://github.com/user-attachments/assets/2a0d7d94-1fc1-4f1e-8681-9b8152a1c813)

Now, to access the application on the Kubernetes Cluster, we take the external IP of the node and the port number. 

https://<External IP>:<go-web-app port number>

![GoProj9](https://github.com/user-attachments/assets/2fed7370-dec1-439f-b2a6-c72d54384353)











