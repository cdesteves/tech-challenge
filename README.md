# LetsGetChecked DevOps Challenge:

## Overview

This repository demonstrates a CI/CD pipeline to deploy a simple web application (using an NGINX Docker image) to a local Kubernetes cluster. The pipeline automates building a Docker image, pushing it to AWS Elastic Container Registry (ECR), and deploying it using Helm to a Minikube Kubernetes cluster.

## Technologies Used

- **Docker**: To build and run the web application (NGINX with a custom `index.html`).
- **Minikube**: Local Kubernetes cluster used for deployment.
- **Helm**: For managing Kubernetes deployments.
- **GitHub Actions**: Automates the CI/CD process, including Docker image builds and Kubernetes deployments.
- **AWS ECR**: Stores the Docker image.
- **Docker Desktop with WSL (Ubuntu)**: Development environment for running containers and Kubernetes on Windows.

## Prerequisites

Before you can run this project locally or interact with the GitHub Actions CI/CD pipeline, ensure you have the following installed and set up:

- **Minikube** (for the local Kubernetes cluster)
- **kubectl** (Kubernetes command-line tool)
- **Helm** (Kubernetes package manager)
- **GitHub account** (to use GitHub Actions)
- **ECR repository** (to store the docker image)
- **If using  Windows**:
  - **WSL** (Ubuntu) 
  - ***Docker Desktop*** (with WSL integration on Windows)

## Project Setup

## Clone the Repository

```bash
git clone https://github.com/cdesteves/tech-challenge.git && cd tech-challenge
```

## Minikube Setup
To deploy the application to your local Kubernetes cluster, start Minikube:

- Start Minikube:
```bash
minikube start
```

## Add ECR credentials

There is the need to add your ECR credentials on:

  - **GitHub Secrets**: so that the pipeline is able to push the new image to the repository.
  - **minikube**: so that the cluster is able to retreive the docker image. 

The credentials on minikube were added using a minikube addon. 
To configure it, run the following command: 
 ```bash
minikube addons configure registry-creds
```

# 052020821647 eu-west-1 -> aws configure no runner
Fill de AWS ECR with your AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY and then enable it via: 
```bash
minikube addons enable registry-creds
```

## CI/CD Pipeline
The GitHub Actions workflow is set up to automate the following steps:

1. Build: Builds the Docker image for the web application.
2. Push: Pushes the Docker image to AWS ECR.
3. Deploy: Uses Helm to deploy the application to the Kubernetes cluster running on Minikube.

The workflow is triggered on any push to the repository. To set up the secrets for AWS ECR, you need to add the following GitHub Secrets:

- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY
- ECR_REPOSITORY (ECR repository URL where the image will be pushed)


GitHub Actions Workflow Overview
The GitHub Actions pipeline (.github/workflows/main.yml) performs the following key steps:

- Checkout: Pulls the code from the repository.
- Build: Builds the Docker image.
- Authenticate: Logs into AWS ECR using credentials stored in GitHub Secrets.
- Push: Pushes the image to ECR.
- Deploy: Uses Helm to deploy the application to the Minikube cluster.

## Accessing the Application
Once the application is deployed to the Kubernetes cluster, you can access it by using the Minikube IP and the service’s NodePort:

bash
Copy code
minikube service nginx-app
This will open the web application in your default browser.

## Customization
You can modify the index.html file to change the content of the NGINX welcome page. The file is located in the root directory and will be copied into the NGINX container during the Docker build process.

## Enhancements
Blue-Green or Canary Deployments
The Helm chart can be extended to support blue-green or canary deployment strategies. For example, you can define multiple Helm values files to manage different versions of the deployment and use Helm hooks to orchestrate the switch between deployments.

Monitoring and Logging
For monitoring and logging, tools like Prometheus and Grafana can be installed in the Minikube cluster. This can help monitor the health and performance of your deployed application.

## Troubleshooting
Minikube Issues: If Minikube fails to start, ensure that Docker Desktop is running and WSL is properly configured.
Helm Deployment: If the Helm deployment fails, check that all Kubernetes resources are correctly configured and that Minikube is running.
ECR Authentication: Ensure that AWS credentials are correctly stored in GitHub Secrets and that the repository permissions are set up for pushing images.
Conclusion
This project showcases a simple CI/CD pipeline for deploying a web application to a local Kubernetes cluster using GitHub Actions, Helm, and AWS ECR. The pipeline automates building, pushing, and deploying the Docker image, while the Helm chart manages the Kubernetes resources.

markdown
Copy code

### Key Sections Breakdown:

- **Overview**: High-level introduction of the project.
- **Technologies Used**: Tools used in the project.
- **Project Setup**: Detailed instructions on how to run the project locally.
- **CI/CD Pipeline**: Information about the GitHub Actions workflow and the steps it automates.
- **Customizations & Enhancements**: Ideas for further improvement.
- **Troubleshooting**: Common issues and their fixes.

Feel free to adapt or modify it based on your preferences!

### 2. Docker Image
This project uses an NGINX Docker image with a custom index.html file that simulates an application. The Docker image is built as part of the CI pipeline and pushed to AWS ECR.

To build and run the Docker image locally:

bash
Copy code
docker build -t my-nginx-app:latest .
docker run -p 8080:80 my-nginx-app:latest
You can now view the web application by navigating to http://localhost:8080.
