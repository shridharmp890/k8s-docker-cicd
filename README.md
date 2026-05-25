# CI/CD Pipeline with Jenkins, Docker & Kubernetes

## Overview
Full end-to-end DevOps pipeline: every GitHub code push triggers Jenkins, which builds a Docker image, pushes it to Docker Hub, and deploys to a Kubernetes cluster with zero-downtime rolling updates and auto-scaling.

## Tools Used
- **Jenkins** — CI/CD pipeline automation
- **Docker** — Containerization + Docker Hub registry
- **Kubernetes** — Container orchestration, HPA, rolling updates
- **Python Flask** — Sample web application

## Project Structure
```
k8s-docker-cicd/
├── app/
│   ├── app.py            # Flask web application
│   └── requirements.txt  # Python dependencies
├── Dockerfile            # Container build instructions
├── k8s/
│   ├── deployment.yaml   # K8s Deployment + HPA config
│   └── service.yaml      # LoadBalancer service
├── Jenkinsfile           # Full CI/CD pipeline
└── README.md
```

## Pipeline Flow
```
GitHub Push
    ↓
Jenkins Triggered (webhook)
    ↓
Docker Build & Tag
    ↓
Push to Docker Hub
    ↓
kubectl apply (Deploy to K8s)
    ↓
Rolling Update (Zero Downtime)
    ↓
HPA monitors CPU/Memory → Auto-scales pods
```

## Kubernetes Features
| Feature | Config |
|---------|--------|
| Rolling Updates | maxSurge: 1, maxUnavailable: 0 |
| Auto-scaling (HPA) | Min: 2 pods, Max: 5 pods |
| CPU threshold | Scale up at 60% |
| Memory threshold | Scale up at 70% |
| Health checks | Readiness + Liveness probes |

## How to Run Manually
```bash
# Build Docker image
docker build -t shridharmp890/devops-app:latest .

# Run locally
docker run -p 5000:5000 shridharmp890/devops-app:latest

# Deploy to Kubernetes
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

# Check status
kubectl get pods
kubectl get hpa
```

## API Endpoints
| Endpoint | Response |
|----------|---------|
| GET / | App info + status |
| GET /health | Health check |
