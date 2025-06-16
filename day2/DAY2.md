# Apply deployment
kubectl apply -f deployment.yaml

# Apply ClusterIP service
kubectl apply -f service.yaml

# Create NodePort service
kubectl expose deployment httpd-deployment --name=httpd-nodeport --type=NodePort --port=80 --target-port=80

# Check deployment status
kubectl rollout status deployment/httpd-deployment -n my-app

# List services
kubectl get services -n my-app

# Port forwarding
kubectl port-forward svc/httpd-nodeport 8080:80 -n my-app

# Test access
curl localhost:8080

# Update image
kubectl set image deployment/httpd-deployment httpd=httpd:2.4.57

# Monitor rollout
kubectl rollout status deployment/httpd-deployment -n my-app