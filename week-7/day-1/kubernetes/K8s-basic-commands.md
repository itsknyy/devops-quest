# Kubernetes Commands - Quick Reference

## Basic Commands

```
# View namespaces
kubectl get namespaces
kubectl get ns  # short version

# Create a deployment
kubectl create deployment my-app --image=httpd

# See all deployments
kubectl get deployments

# Get detailed info about a deployment
kubectl describe deployment my-app

# See all pods
kubectl get pods

# Get detailed info about a pod
kubectl describe pod <pod_name>

# Expose a service (make it accessible)
kubectl expose deployment my-app --type=NodePort --port=80

# See all services
kubectl get services
kubectl get svc  # short version

# Get detailed info about a service
kubectl describe service my-app

# Delete resources
kubectl delete deployment my-app
kubectl delete service my-app
```
---
