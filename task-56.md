> Create a Deployment 
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
---
> Create a NodePort Service for the Deployment
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