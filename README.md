# Devops_Final
## Project Overview
A cloud-native e-commerce platform built with microservices architecture,
containerized with Docker, orchestrated with Kubernetes, and deployed via
CI/CD pipelines using GitHub Actions.

## Team
| Name | Roll Number | Role | Page |
|------|-------------|------|------|
| Muhammad Ibrahim Raza | FA23-BCS-121 | Team Lead | Home Page , Account Page , Product Page |
| Muhammad Ali Raza | FA23-BCS-101 | Team Member | Notification Page |
| Muhammad Ibrahim Raza | FA23-BCS-121 | Team Member | Order Page  |


## Repository URLs
- **GitHub Repo:** https://github.com/Ibrahim29087/Devops_Final
- **Docker Hub:** https://hub.docker.com/u/ibrahim29087

## Live Environments
| Environment | URL |
|---|---|
| Production | https://ecommerce-production.onrender.com |
| Staging | https://ecommerce-staging.onrender.com |
| Development | https://ecommerce-development.onrender.com |

## Folder Structure
project/
├── .github/workflows/     # CI/CD pipelines
│   ├── ci-development.yml
│   ├── ci-staging.yml
│   └── cd-production.yml
├── Dockerfile             # Frontend containerization
├── Jenkinsfile            # Jenkins pipeline
├── README.md
├── src/
│   ├── frontend/          # Main dashboard
│   ├── user-service/      # User management
│   ├── product-service/   # Product catalog
│   ├── order-service/     # Order processing
│   └── notification-service/ # Notifications
└── k8s/
├── frontend-deployment.yaml
├── user-deployment.yaml
├── product-deployment.yaml
├── order-deployment.yaml
└── notification-deployment.yaml

## CI/CD Pipeline
- **Development:** Auto-deploys on push to `develop` branch
- **Staging:** Auto-deploys on push to `staging` branch  
- **Production:** Auto-deploys on push to `main` branch (requires approval)

## Pipeline Stages
1. Checkout Code
2. Lint HTML Files
3. Run Unit Tests
4. Docker Login
5. Build & Push Docker Image
6. Deploy to Render

## Docker Images
| Service | Image |
|---|---|
| Frontend | `ibrahim29087/frontend:latest` |

## Kubernetes Deployments
Each service deployed with:
- Deployment with 2 replicas
- ClusterIP Service
- Rolling update strategy

## How to Run Locally

### Using Docker
```bash
docker pull ibrahim29087/frontend:latest
docker run -p 80:80 ibrahim29087/frontend:latest
```

### Using Kubernetes
```bash
kubectl apply -f k8s/
kubectl get pods
kubectl get services
```

## Secrets Management
All secrets stored in GitHub Environment Secrets:
- `DOCKER_USERNAME` / `DOCKER_PASSWORD`
- `RENDER_API_KEY`
- `RENDER_SERVICE_ID_PROD/STAGING/DEV`

## Reflection
This project provided hands-on experience with containerized microservices
and Kubernetes orchestration. Implementing CI/CD pipelines taught us how
enterprise-grade DevOps workflows operate. Key challenges included managing
environment-specific secrets, configuring multi-branch deployments, and
debugging Docker build failures in GitHub Actions.
