# TASK: Deploy Custom Docker Image from a Private Registry to Kubernetes

**Objective**

Deploy a custom-built Docker image stored in a private container registry as a Kubernetes Deployment and expose it using a Kubernetes Service:

**Prerequisites**

- Docker running 
- Private container registry (Docker Hub)
- Kubernetes cluster (Minikube)

---

**STEP-1: Create a private repository in DockerHub**

![images](../images/image-1.png)

---

**STEP-2: Pulling NGINX Image from DockerHub**

![images](../images/image-2.png)

---

**STEP-3: Tag Image for Private Registry**

![images](../images/image-3.png)

---

**STEP-4: Push the image to private registry**

```
docker push itsknyy/k8s-private-test:latest
```

![images](../images/image-4.png)

---

**STEP-5: Create a namespace in kubernetes**

```
kubectl create ns docker-k8s
```

![images](../images/image-5.png)

---

**STEP-6: Create Kubernetes ImagePullSecret**

```
kubectl create secret docker-registry my-secret --docker-server=https://index.docker.io/v1/ --docker-username=itsknyy --docker-password=**************** --docker-email=contact.kenish@gamil.com --namespace=docker-k8s
```
- Verify:

![images](../images/image-6.png)

---

**STEP-7: Kubernetes Deployment YAML**

```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx-private
  name: nginx-private
  namespace: docker-k8s
spec: 
  replicas: 1
  selector:
    matchLabels:
      app: nginx-private
  template:
    metadata:
      labels:
        app: nginx-private
    spec:
      imagePullSecrets:             
      - name: my-secret                
      containers:
      - name: nginx-private
        image: itsknyy/k8s-private-test:latest
        ports:
        - containerPort: 80
```

- Apply:
```
kubectl apply -f deployment.yaml
```
- Check Status:
```
kubectl get pods -n docker-k8s
```
![images](../images/image-7.png)

---

**STEP-8: Expose the Deployment via Service**

![images](../images/image-8.png)

---

**STEP-9: Access the Application**

![images](../images/image-9.png)

---

**STEP-10: Verify**

![images](../images/image-10.png)

---


