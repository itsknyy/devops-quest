# Workshop: Container Orchestration

## Setup
- Install minikube on local
- Start cluster: `minikube start`
- Docker Desktop should be running.

## Deploy Public Image

**Step 1: Check namespaces**

```
kubectl get namespaces
```

![alt text](../images/image-0.png)

---

**Step 2: Create deployment with public image**

```
kubectl create deployment my-app-1 --image=httpd

# Uses httpd image from Docker Hub
# Goes to 'default' namespace automatically
```
![alt text](../images/image-1.png)

---

**Step 3: Check deployment**

```
kubectl check deployments
kubectl describe deplyment my-app-1
```
![alt text](../images/image-2.png)

---

**Step 4: Check the pod**

```
kubectl get pods
kubectl describe pod <pod_name>
```

---

**Step 5: Expose the service**

```
kubectl expose deployment my-app-1 --type=NodePort --port=80
```

![alt text](../images/image-3.png)

---

**Step 6: Check the service**

```
kubectl get services
kubectl describe service my-app-1
```
![alt text](../images/image-4.png)

---

**Step 7: Access the app**

```
minikube service my-app-1
```
![alt text](../images/image-5.png)

![alt text](../images/image-6.png)

---