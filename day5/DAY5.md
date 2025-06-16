```bash
#View container logs
kubectl logs <pod-name> [-c container-name]
kubectl logs -f <pod-name> # Follow logs
  
#resources info
kubectl describe pod <pod-name>
kubectl describe deployment <deployment-name>

#cluster events
kubectl get events --sort-by='.lastTimestamp'

#Execute commands in container
kubectl exec -it <pod-name> -- /bin/sh

#create workload to test hpa 
kubectl run -i --tty load-generator --rm --image=busybox:1.28 --restart=Never -- /bin/sh -c "while true; do wget -q -O- http://<service-name>:<port>; done"