## Day 56: Deploy Nginx Web Server on Kubernetes Cluster 

1- Create Nginx Deployment
> Create file `nginx-deployment.yaml` using `vi nginx-deployment.yaml` and add the following content:
``` yaml
---
kind: Deployment 
apiVersion: apps/v1
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-deployment
  template:
    metadata:
      name: nginx-pod
      labels:
        app: nginx-deployment
    spec:
      containers:
        - name: nginx-container
          image: nginx:latest
          ports:
            - containerPort: 80
```
> save and exit the file.
>
> Apply the Deployment using the command:
```bash
kubectl apply -f nginx-deployment.yaml
```
---
2- Create Nginx Service
> Create file `nginx-service.yaml` using `vi nginx-service.yaml` and add the following content:
```yaml
---
kind: Service
apiVersion: v1
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx-deployment
  type: NodePort
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30011
```
> save and exit the file.
> 
> Apply the Service using the command:
```bash
kubectl apply -f nginx-service.yaml
```
---
3- Verify the Deployment and Service
> Verify the Deployment using the command:
``` bash  
kubectl get deployments
kubectl get services
```