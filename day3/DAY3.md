```bash
## Deployment Process

### 1. Install Helm
choco install kubernetes-helm

# Verify installation
helm version

### 2. Deploy WordPress
# Add Bitnami repository
helm repo add bitnami https://charts.bitnami.com/bitnami

# Install WordPress
helm install my-wp bitnami/wordpress --namespace my-app


### 3. Apply Configuration
# Apply ConfigMap
kubectl apply -f configmap.yaml

# Apply Secret
kubectl apply -f secret.yaml


#package
helm package my-app-chart
docker login
helm push <filename.tgz> oci://registry-1.docker.io/ptime123

#list available versions
helm show chart oci://registry-1.docker.io/ptime123/my-app --version 0.2.0

#pull the chart
helm pull oci://registry-1.docker.io/ptime123/my-app --version 0.2.0

#update or install chart
helm install my-release oci://registry-1.docker.io/ptime123/my-app --version 0.2.0
helm upgrade my-release oci://registry-1.docker.io/ptime123/my-app --version 0.2.0

### 2. Verify Configuration
# List ConfigMaps and Secrets
kubectl get configmaps,secrets -n my-app

# Check ConfigMap contents
kubectl describe configmap app-config -n my-app

# Check Secret contents (encoded)
kubectl describe secret db-credentials -n my-app

### 3. Test Application
# Port forward WordPress
kubectl port-forward svc/my-wp 8080:80 -n my-app

# Access application
curl localhost:8080

