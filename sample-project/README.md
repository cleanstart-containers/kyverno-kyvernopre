# Kyverno-Kyvernopre Sample Project

This sample project demonstrates how to properly run and test the Kyverno-Kyvernopre container with the correct environment variables and configurations.

## 🚀 Quick Start - Working Steps

### Step 1: Pull the Image
```bash
docker pull cleanstart/kyverno-kyvernopre:latest
```

### Step 2: Test Container (Recommended)
```bash
docker run --rm --name kyverno-test \
  -e KYVERNO_NAMESPACE=kyverno \
  -e KYVERNO_SERVICEACCOUNT_NAME=kyverno \
  -e KYVERNO_DEPLOYMENT=kyverno \
  -e KYVERNO_POD_NAME=kyverno-pre \
  -e INIT_CONFIG='{"config":{}}' \
  -e METRICS_CONFIG='{"enabled":false}' \
  cleanstart/kyverno-kyvernopre:latest --help
```

### Step 3: Run Production Container (requires Kubernetes API access)

Kyverno needs a Kubernetes API server. It first tries **in-cluster** config (`KUBERNETES_SERVICE_HOST` / `KUBERNETES_SERVICE_PORT`). When you run the container with plain `docker run` (not inside a cluster), those are not set and you get:

```text
failed to create rest client configuration error="unable to load in-cluster configuration, KUBERNETES_SERVICE_HOST and KUBERNETES_SERVICE_PORT must be defined"
```

**Option A – Run inside a Kubernetes cluster (recommended)**  
Use the manifests under `../kubernetes - GKE/` (namespace, service account, deployment, service) and deploy to your cluster so the container runs as a pod. Then it gets in-cluster config automatically.

**Option B – Run with Docker using a kubeconfig**  
Mount a kubeconfig that can reach your cluster (Kind, Minikube, or remote) and set `KUBECONFIG`:

```bash
docker run -d --name kyverno-prod \
  -e KYVERNO_NAMESPACE=kyverno \
  -e KYVERNO_SERVICEACCOUNT_NAME=kyverno \
  -e KYVERNO_DEPLOYMENT=kyverno \
  -e KYVERNO_POD_NAME=kyverno-pre \
  -e INIT_CONFIG='{"config":{}}' \
  -e METRICS_CONFIG='{"enabled":false}' \
  -e KUBECONFIG=/app/.kube/config \
  -v "$HOME/.kube/config:/app/.kube/config:ro" \
  cleanstart/kyverno-kyvernopre:latest
```

Adjust the mount path if your kubeconfig lives elsewhere (e.g. Minikube/Kind often use `~/.kube/config`). The container must be able to reach the API server (use the same kubeconfig you use with `kubectl`).

**Summary:** Step 3 only works when the container can reach a Kubernetes API server—either by running as a pod (Option A) or by using a mounted kubeconfig (Option B).

### Step 4: Check Container Status
```bash
docker logs kyverno-prod
```

### Step 5: Clean Up
```bash
docker stop kyverno-prod
docker rm kyverno-prod
```

## 🔧 Required Environment Variables

The container requires these 6 environment variables to start properly:

| Variable | Value | Description |
|----------|-------|-------------|
| `KYVERNO_NAMESPACE` | kyverno | Namespace where Kyverno is installed |
| `KYVERNO_SERVICEACCOUNT_NAME` | kyverno | Service account name for Kyverno |
| `KYVERNO_DEPLOYMENT` | kyverno | Deployment name for Kyverno |
| `KYVERNO_POD_NAME` | kyverno-pre | Pod name for Kyverno pre-hook |
| `INIT_CONFIG` | `{"config":{}}` | Initial configuration (JSON object) |
| `METRICS_CONFIG` | `{"enabled":false}` | Metrics configuration (JSON object) |

## 📁 Files in This Project

- `README.md` - This documentation with working steps
- `Dockerfile` - Sample Dockerfile showing how to extend the base image
- `test-config.yaml` - Sample Kyverno policy configuration

## 🎯 What This Container Does

The Kyverno-Kyvernopre container is a Kubernetes admission controller that:
- Validates Kubernetes resources before they're created
- Supports various configuration options for logging, rate limiting, and metrics
- Requires a Kubernetes cluster connection for full operation
- Can be tested using the `--help` flag to show available options

## 💡 Tips

- Use `--help` command for testing (no Kubernetes cluster required)
- The container runs as an admission controller in Kubernetes
- All environment variables are required for proper startup
- Use `--rm` flag for testing to automatically clean up containers