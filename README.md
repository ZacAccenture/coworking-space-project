# Coworking Space Service

## Overview

This project operationalizes a Python analytics microservice for a coworking space. The service provides analytics on coworking-space usage and is containerized with Docker, built through AWS CodeBuild, stored in Amazon ECR, deployed to Amazon EKS, and monitored using Amazon CloudWatch Container Insights.

## Architecture

The deployment uses the following technologies:

- **Python / Flask** for the analytics API.
- **PostgreSQL** for application data.
- **Docker** to package the analytics application.
- **AWS CodeBuild** for continuous integration and Docker image creation.
- **Amazon ECR** to store versioned Docker images.
- **Amazon EKS / Kubernetes** to run PostgreSQL and the analytics microservice.
- **Kubernetes ConfigMap** for non-sensitive database configuration.
- **Kubernetes Secret** for the database password.
- **Kubernetes LoadBalancer Service** to expose the analytics API.
- **Amazon CloudWatch Container Insights** to collect application and Kubernetes logs and metrics.

## Build and Release Process

Application source code is stored in GitHub. AWS CodeBuild uses `buildspec.yaml` to build the Docker image and push it to the `coworking-analytics` Amazon ECR repository.

Docker images use semantic versioning. The CodeBuild build number is incorporated into the image version, producing versions such as:

`1.0.4`  
`1.0.5`

To release a new version:

1. Update and test the application code.
2. Commit and push the changes to the GitHub repository.
3. Start the AWS CodeBuild project.
4. Confirm the build succeeds and the new image appears in Amazon ECR.
5. Update the image version in `deployments/coworking.yaml`.
6. Apply the updated Kubernetes configuration.
7. Confirm the deployment successfully rolls out.
8. Verify the API and CloudWatch logs.

Example deployment commands:

```bash
kubectl apply -f deployments/configmap.yaml
kubectl apply -f deployments/secret.yaml
kubectl apply -f deployments/coworking.yaml
kubectl rollout status deployment/coworking
kubectl get pods
kubectl get svc