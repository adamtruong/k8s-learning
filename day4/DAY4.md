## Deployment Architecture
1. Two worker nodes and one control plane
2. Gateway Node (Worker 1):
   - Acts as internet gateway/NAT
   - Only runs Nginx Ingress Controller
   - Locked from other pods using taints
   - Handles external traffic
3. Private Node (Worker 2):
   - Runs application workloads
   - Network traffic only through gateway node
   - One-way traffic (private subnet)

### Expected Output
1. Nginx pod running with PVC:
   - `kubectl get pvc` shows bound claim
   - Data persists after pod restart
   - Index.html remains unchanged
2. Ingress Controller:
   - Running on gateway node
   - `kubectl get pods -n ingress-nginx` shows pods
3. Routing:
   - `/app` routes to Day 2 Apache service
   - `curl localhost:8080/app` returns Apache page

```bash
## Setup Process
### 1. Install Ingress Controller
# Add ingress-nginx repository
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

#delete and install the new configured cluster
kind delete cluster
kind create cluster --config day4/cluster-config.yaml
kubectl get nodes --show-labels

# Install ingress-nginx
helm install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx \
  --create-namespace

### 2. Deploy Storage and Application
# Create PVC
kubectl apply -f pvc.yaml

# Deploy Nginx with PVC
kubectl apply -f nginx-deployment.yaml

# Create Ingress rules
kubectl apply -f ingress.yaml

### 3. Verify Setup
# Check PVC status
kubectl get pvc -n my-app

# Verify pods
kubectl get pods -n my-app

# Check ingress controller
kubectl get pods -n ingress-nginx

# Test routing
kubectl port-forward svc/ingress-nginx-controller 8080:80 -n ingress-nginx
curl localhost:8080/
curl localhost:8080/app

