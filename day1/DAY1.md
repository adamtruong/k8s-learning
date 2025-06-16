Cluster Configuration:
- 1 control plane node
- 2 worker nodes
- Port mappings for HTTP (80) and HTTPS (443)

#create cluster
kind create cluster --name my-cluster --config kind-config.yaml

# Verify cluster
kubectl get nodes

# Create namespace
kubectl create namespace my-app

# List namespaces
kubectl get namespaces

# Get pods in namespace
kubectl get pods -n my-app

# Describe pod details
kubectl describe pod nginx-pod -n my-app

# Get pod logs
kubectl logs nginx-pod -n my-app

# Execute command in pod
kubectl exec -it nginx-pod -n my-app -- /bin/bash

# Verify cluster nodes
kubectl get nodes -n my-app

# Verify pod
kubectl get pods -n my-app
kubectl describe pod nginx-pod -n my-app