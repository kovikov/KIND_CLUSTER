# Kubernetes Deployment: Minikube or Kind

This repo contains Kubernetes manifests to deploy your DockerHub project image.

## Files

- `deployment.yml` - Creates a Deployment with 2 replicas
- `service.yml` - Exposes the app as a NodePort Service

## Prerequisites

- Docker Desktop installed and running
- `kubectl` installed
- One of:
  - `minikube` (recommended for this guide), or
  - `kind`

### Windows install commands (PowerShell)

```powershell
winget install Kubernetes.minikube
winget install Kubernetes.kind
```

Verify:

```powershell
minikube version
kind version
kubectl version --client
```

## 1) Build and Push Your Project Image to DockerHub

From your project source directory:

```bash
docker login
docker build -t your-dockerhub-username/your-project:latest .
docker push your-dockerhub-username/your-project:latest
```

Update `deployment.yml` image value:

```yaml
image: your-dockerhub-username/your-project:latest
```

## 2) Start a Cluster

### Option A: Minikube

```bash
minikube start --driver=docker
kubectl config use-context minikube
```

### Option B: Kind

```bash
kind create cluster --name my-kind
kubectl cluster-info --context kind-my-kind
```

## 3) Deploy to Minikube (or current kubectl context)

From this repo directory:

```bash
kubectl apply -f deployment.yml
kubectl apply -f service.yml
kubectl get pods
kubectl get svc
```

## 4) Access the Application

### If using Minikube

```bash
minikube service my-app-service --url
```

### If using Kind (NodePort)

Use Docker Desktop/Windows networking to reach NodePort if available, or use port-forward:

```bash
kubectl port-forward service/my-app-service 8080:80
```

Open:

```text
http://localhost:8080
```

## 5) Verify Rollout and Logs

```bash
kubectl rollout status deployment/my-app-deployment
kubectl logs deployment/my-app-deployment
```

## 6) Cleanup

```bash
kubectl delete -f service.yml
kubectl delete -f deployment.yml
minikube stop
# or for kind:
# kind delete cluster --name my-kind
```

## 7) Document and Share on GitHub

From this repo directory:

```bash
git init
git add .
git commit -m "Add Kubernetes deployment and service manifests with setup guide"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

In your GitHub repo, include:

- DockerHub image link
- Screenshot of `kubectl get pods` and `kubectl get svc`
- App URL output (`minikube service ... --url` or localhost port-forward URL)
- Short notes on any issue you fixed
# KIND_CLUSTER
