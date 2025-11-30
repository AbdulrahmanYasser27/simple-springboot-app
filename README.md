# Simple Spring Boot App - DevOps Assignment

This repository contains a simple Spring Boot backend application, containerized using Docker, and deployed on a local Kubernetes cluster (Minikube). It also includes a CI/CD pipeline using GitHub Actions.

---

## 1. Running Locally

1. Clone the repository:

```bash
git clone https://github.com/<your-username>/simple-springboot-app.git
cd simple-springboot-app
```

2. Build the Docker image:

```bash
docker build -t abdulrahman1235/simple-backend:latest .
```

3. Run the container locally:

```bash
docker run -p 8080:8080 abdulrahman1235/simple-backend:latest
```

4. Test in browser or curl:

```bash
curl http://localhost:8080
```

## 2. Deploying to Kubernetes (Minikube)

### Prerequisites

- Minikube installed and running
- `kubectl` installed
- Docker CLI or Docker Desktop configured
- Ingress addon enabled:

```bash
minikube addons enable ingress
```

### Namespaces

Create `dev` and `prod` namespaces:

```bash
kubectl create namespace dev
kubectl create namespace prod
```

### Apply Kubernetes manifests

# Deploy to dev
```bash
kubectl apply -f k8s/deployment.yaml -n dev
kubectl apply -f k8s/service.yaml -n dev
kubectl apply -f k8s/ingress.yaml -n dev
```

# Test dev
```bash
curl http://simple-dev.local
```

# Deploy to prod (after dev is healthy)
```bash
kubectl apply -f k8s/deployment.yaml -n prod
kubectl apply -f k8s/service.yaml -n prod
kubectl apply -f k8s/ingress.yaml -n prod
```

# Test prod
```bash
curl http://simple-prod.local
```

#### Hosts file configuration

Add the following lines to your Windows hosts file (`C:\Windows\System32\drivers\etc\hosts`):

```
192.168.49.2 simple-dev.local
192.168.49.2 simple-prod.local
```

## 3. CI/CD Pipeline (GitHub Actions)

### CI (Continuous Integration)

- Lint the code (if applicable)
- Run unit tests
- Build Docker image
- Push Docker image to Docker Hub (`abdulrahman1235/simple-backend:latest`)

### CD (Continuous Deployment)

- Deploy the latest Docker image to the `dev` namespace in Minikube.
- Perform a basic smoke/health check using `curl`.
- If successful, deploy the same image to the `prod` namespace.

Note: Since Minikube is local, CD is executed on your machine.

## 4. Assumptions

- Application runs on port 8080.
- Docker image is publicly accessible on Docker Hub.
- Minikube IP may change, so hosts file must be updated accordingly.
- GitHub Actions workflow only handles CI (build + push Docker image).

## 5. Prerequisites

- Docker
- Minikube
- kubectl
- GitHub account and Docker Hub account
- Windows hosts file editable (admin privileges)