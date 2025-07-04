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



## Running the server

To run the server, execute the following command:

```bash
go run main.go
```

The server will start on port 8080. You can access it by navigating to `http://localhost:8080/courses` in your web browser.

## Looks like this

![Website](static/images/golang-website.png)


