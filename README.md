# DevOps App Deployment

A simple Flask application with Docker and Kubernetes configurations for learning and demonstration purposes.

## Overview

This project showcases containerization and orchestration of a minimal Python Flask web service using Docker and Kubernetes. It serves as a practical example of how to deploy applications in a cloud-native environment.

## Features

- **Flask Web Application** - Simple HTTP server with `/` and `/health` endpoints
- **Docker Containerization** - Production-ready Docker image based on Python 3.9-slim
- **Kubernetes Deployment** - Complete K8s manifests including Deployment, Service, and Ingress
- **Health Checks** - Built-in health endpoint for monitoring

## Project Structure

```
├── Dockerfile              # Container image definition
├── app.py                 # Flask application entry point
├── requirements.txt       # Python dependencies
└── kubernetes/            # Kubernetes manifests
    ├── deployment.yaml    # Deployment configuration (2 replicas)
    ├── service.yaml       # NodePort service (port 30007)
    └── ingress.yaml       # Ingress routing (devops.local)
```

## Quick Start

### Prerequisites

- Python 3.9+
- Docker (for containerized setup)
- Kubernetes cluster + kubectl (for K8s deployment)

### Local Development

```bash
# Install dependencies
pip install -r requirements.txt

# Run the Flask app
python app.py

# Access the app
curl http://localhost:5000
curl http://localhost:5000/health
```

### Docker

```bash
# Build the image
docker build -t devops-app:v1 .

# Run the container
docker run -p 5000:5000 devops-app:v1

# Test the app
curl http://localhost:5000
```

### Kubernetes Deployment

```bash
# Apply all manifests
kubectl apply -f kubernetes/

# Check deployment status
kubectl get pods
kubectl get svc

# Port-forward to test (alternative to Ingress)
kubectl port-forward svc/devops-service 8000:80

# Access via Ingress (requires ingress controller)
# Add to /etc/hosts: 127.0.0.1 devops.local
curl http://devops.local
```

## API Endpoints

| Endpoint | Method | Response | Purpose |
|----------|--------|----------|---------|
| `/` | GET | `Hello DevOps 🚀` | Home endpoint |
| `/health` | GET | `OK` (200) | Health check |

## Configuration

### Container Configuration
- **Base Image:** Python 3.9-slim
- **Port:** 5000
- **Working Directory:** `/app`

### Kubernetes Configuration
- **Replicas:** 2
- **Service Type:** NodePort
- **Service Port:** 80 → 5000 (internal)
- **Node Port:** 30007
- **Ingress Host:** devops.local

## Deployment Architecture

```
Internet
   ↓
[Ingress (devops.local)]
   ↓
[Service (NodePort 30007)]
   ↓
[Deployment (2 replicas)]
   ↓
[Pod 1 - Flask App] [Pod 2 - Flask App]
```

## Requirements

See `requirements.txt` for Python dependencies:
- flask

## License

MIT

## Author

Fabinwilfred
