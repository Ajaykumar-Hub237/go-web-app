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

To access the application on the Kubernetes Cluster, we need to obtain the external IP address of the node and the port number. 

https://<External IP>:<go-web-app port number>/courses

![image](https://github.com/user-attachments/assets/e05c217c-0816-4243-bf9e-8aff6308025f)

## Install Nginx Ingress Controller On AWS
This will create a Network Load Balancer. It can be done using the command below:

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.11.1/deploy/static/provider/aws/deploy.yaml
```

![GoProj10](https://github.com/user-attachments/assets/96230424-26ea-4fb0-8efd-7a3339fb8122)

Next, to ensure the Ingress Controller watches the Ingress Resource, we use the command:

```
kubectl get ing
```

It gives us the FQDN, the Fully Qualified Domain Name. 
![GoProj11](https://github.com/user-attachments/assets/3e4d8f83-54f3-45cb-bcdc-dd8f5f93e5d1)

## DNS Mapping

To get the IP address of the load balancer, we use the command: 

```
nslookup <DNS name>
```

Next, we proceed to map the DNS with the IP address, using the command below:

```
sudo vim /etc/hosts/
```

![GoProj12](https://github.com/user-attachments/assets/7d5ae032-b060-44ae-8636-11c4bb470c07)

## Helm Package Setup
Let's install Helm first on our machine.

**For Windows:**

**Using Chocolatey:**

First, install Chocolatey if you haven't already. Open an administrative Command Prompt and run:

```
@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```

**Install Helm:**

```
choco install Kubernetes-helm
```

**Verify Installation:**

```
helm version
```

**For Linux:**

**Download Helm**

```
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

**Verify Installation:**

```
helm version
```

## To Create A Directory Structure For The Helm Chart:

In the project folder, create a new directory, Helm, and switch to it.

```mkdir Helm
   cd Helm
```

In the Helm Folder, create

```
helm create go-web-app-chart
```

Deploy using Helm chart in /go-web-app-chart/helm:

```
helm install go-web-app ./go-web-app-chart
```

![GoProj13](https://github.com/user-attachments/assets/93d27584-aee8-4598-8905-b63bcfa79ea7)

## To Uninstall Helm: 

```
helm uninstall go-web-app
```

![GoProj14](https://github.com/user-attachments/assets/6b5f704c-4bb0-45bd-9f63-13286a2e7609)

## Implementing CI Using GitHub Actions:

**Multiple Stages of CI:**

**-Build and Unit Test**

**-Static Code Analysis**

**-Create Docker Image and Push**

**-Update Helm for every commit made(values.yaml)**

**In the project folder:**
-Create this: .github\workflows\ci.yaml

## Continuous Integration Workflow
This project includes a CI workflow defined in the following YAML file:

[ci.yaml](https://github.com/SamuelUdeh/go-web-app/blob/main/.github/workflows/ci.yaml)

```
name: CI/CD

on:
  push:
    branches:
      - main
    paths-ignore:
      - 'helm/**'
      - 'k8s/**'
      - 'README.md'

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout repository
      uses: actions/checkout@v4
    - name: Set up Go 1.21
      uses: actions/setup-go@v5
      with:
        go-version: 1.21
    - name: Build
      run: go build -o go-web-app
    - name: Test
      run: go test ./...

  code-quality:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout repository
      uses: actions/checkout@v4
    - name: Set up Go 1.21
      uses: actions/setup-go@v5
      with:
        go-version: 1.21
    - name: Run golangci-lint
      uses: golangci/golangci-lint-action@v6
      with:
        version: v1.59.1
        args: --timeout 5m

  push:
    runs-on: ubuntu-latest
    needs: build
    steps:
    - name: Checkout repository
      uses: actions/checkout@v4
    - name: Set up Go 1.21
      uses: actions/setup-go@v5
      with:
        go-version: 1.21
    - name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v2
    - name: Login to DockerHub
      uses: docker/login-action@v3
      with:
        username: ${{ secrets.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_TOKEN }}
    - name: Build and Push action
      uses: docker/build-push-action@v6
      with:
        context: .
        file: ./Dockerfile
        push: true
        tags: ${{ secrets.DOCKERHUB_USERNAME }}/go-web-app:${{github.run_id}}

  update-newtag-in-helm-chart:
    runs-on: ubuntu-latest
    needs: push
    steps:
    - name: Checkout repository
      uses: actions/checkout@v4
      with:
        token: ${{ secrets.TOKEN }}
    - name: Update tag in Helm chart
      run: |
        sed -i 's/tag: .*/tag: "${{github.run_id}}"/' helm/go-web-app-chart/values.yaml
    - name: Commit and push changes
      run: |
        git config --global user.email "samuel.udeh.14@gmail.com"
        git config --global user.name "SamuelUdeh"
        git add helm/go-web-app-chart/values.yaml
        git commit -m "Update tag in Helm chart"
        git push
```


















