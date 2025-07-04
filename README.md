# End-to-End CI/CD Pipeline for A Go Web App On AWS EKS with GitHub Actions, Argo CD & Helm

This project demonstrates a fully automated, production-grade CI/CD pipeline for a Go (Golang) web application. It uses Docker, GitHub Actions for continuous integration, Argo CD for GitOps-based continuous delivery, and Helm to manage Kubernetes manifests on AWS EKS.

## Running the server

To run the server, execute the following command:

```bash
go run main.go
```

The server will start on port 8080. You can access it by navigating to `http://localhost:8080/courses` in your web browser.

## Looks like this

![Website](static/images/golang-website.png)


