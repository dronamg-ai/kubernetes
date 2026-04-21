# Kubernetes Infrastructure

This repository contains Kubernetes manifests for managing multi-environment deployments across dev, QA, and staging environments.

## Project Structure

```
├── namespaces.yaml          # Kubernetes namespaces (dev, qa, staging)
├── dev-deployment.yaml      # Dev environment deployment
├── dev-service.yaml         # Dev environment service
├── qa-deployment.yaml       # QA environment deployment
├── qa-service.yaml          # QA environment service
├── staging-deployment.yaml  # Staging environment deployment
├── staging-service.yaml     # Staging environment service
└── README.md               # This file
```

## Environments

### Development (dev)
- **Replicas**: 1
- **Image**: nginx:latest
- **Service Type**: NodePort
- **Purpose**: Local development and testing
- **Access**: NodePort for easy local access

### QA (qa)
- **Replicas**: 2
- **Image**: nginx:stable
- **Service Type**: LoadBalancer
- **Purpose**: Quality assurance and testing
- **Access**: External LoadBalancer IP

### Staging (staging)
- **Replicas**: 2
- **Image**: nginx:latest
- **Service Type**: LoadBalancer
- **Purpose**: Pre-production environment
- **Access**: External LoadBalancer IP

## Prerequisites

- Docker Desktop with Kubernetes enabled
- kubectl CLI installed and configured
- Git for version control

## Installation

### 1. Enable Kubernetes in Docker Desktop

1. Open Docker Desktop
2. Go to **Settings** → **Kubernetes**
3. Check **Enable Kubernetes**
4. Click **Apply & Restart**
5. Wait for Kubernetes to fully initialize (2-5 minutes)

### 2. Verify Kubernetes Cluster

```powershell
kubectl cluster-info
kubectl get nodes
```

### 3. Clone the Repository

```bash
git clone https://github.com/dronamg-ai/kubernetes.git
cd kubernetes
```

## Deployment

### Deploy All Namespaces

```bash
kubectl apply -f namespaces.yaml
```

### Deploy Dev Environment

```bash
kubectl apply -f dev-deployment.yaml dev-service.yaml
```

### Deploy QA Environment

```bash
kubectl apply -f qa-deployment.yaml qa-service.yaml
```

### Deploy Staging Environment

```bash
kubectl apply -f staging-deployment.yaml staging-service.yaml
```

### Deploy All Environments

```bash
kubectl apply -f .
```

## Verification

### Check Namespaces

```bash
kubectl get namespaces
```

### Check Deployments

```bash
kubectl get deployments -A
```

### Check Pods

```bash
kubectl get pods -A
```

### Check Services

```bash
kubectl get services -A
```

### Check Specific Environment (e.g., staging)

```bash
kubectl get all -n staging
```

## Accessing Services

### Dev Environment (NodePort)

```bash
kubectl get svc web-service -n dev
# Access via: localhost:<NODE_PORT>
```

### QA Environment (LoadBalancer)

```bash
kubectl get svc web-service -n qa
# Access via: <EXTERNAL_IP>
```

### Staging Environment (LoadBalancer)

```bash
kubectl get svc web-service -n staging
# Access via: <EXTERNAL_IP>
```

## Managing Deployments

### Scale a Deployment

```bash
kubectl scale deployment web-server -n staging --replicas=3
```

### View Logs

```bash
kubectl logs -n staging deployment/web-server
```

### Describe Pod

```bash
kubectl describe pod <POD_NAME> -n staging
```

### Port Forward

```bash
kubectl port-forward -n staging svc/web-service 8080:80
# Access via: http://localhost:8080
```

## Cleanup

### Delete Specific Environment

```bash
kubectl delete namespace staging
```

### Delete All Resources

```bash
kubectl delete namespace dev qa staging
```

## Troubleshooting

### Pods not starting (ErrImagePull)
- Check internet connection
- Verify Docker Hub accessibility
- Pull image manually: `docker pull nginx:latest`

### Service not accessible
- Verify service type and port configuration
- Check if pods are running: `kubectl get pods -n <namespace>`
- Verify network policies and firewall rules

### kubectl not connecting to cluster
```bash
kubectl config current-context
kubectl config view
```

## Contributing

1. Create a new branch for changes
2. Test changes locally
3. Commit and push to GitHub
4. Create a pull request for review

## License

This project is provided as-is for infrastructure management purposes.

## Support

For issues or questions, please open an issue on GitHub or contact the infrastructure team.
