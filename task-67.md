## Day 67: Deploy Guest Book App on Kubernetes

 1- Create redis deployment

> Create file `redis-master-deployment.yaml` using `vi redis-master-deployment.yaml` and add the following content:
``` yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-master
  labels:
    app: redis-m
spec:
    replicas: 1
    selector:
        matchLabels:
            app: redis-m
    template:
        metadata:
            labels:
                app: redis-m
        spec:
            containers:
                - name: master-redis-devops
                  image: redis:latest
                  ports:
                    - containerPort: 6379
                  resources:
                    requests:
                        memory: "100Mi"
                        cpu: "100m"
```
> Save and exit the file.
> Apply the deployment using the command:
``` bash
kubectl apply -f redis-master-deployment.yaml
```
---
2- Create redis service
> Create file `redis-service.yaml` using `vi redis-service.yaml` and add the following content:
``` yaml
apiVersion: v1
kind: Service
metadata:
  name: redis-master
spec:
  selector:
    app: redis-m
  ports:
    - protocol: TCP
      port: 6379
      targetPort: 6379
```
> Save and exit the file.
> Apply the service using the command:
``` bash
kubectl apply -f redis-service.yaml
```
---
3- Create another deployment for redis slave
> Create file `redis-slave-deployment.yaml` using `vi redis-slave-deployment.yaml` and add the following content:
``` yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-slave
  labels:
    app: redis-s 
spec:
    replicas: 2
    selector:
        matchLabels:
            app: redis-s
    template:
        metadata:
            labels:
                app: redis-s
        spec:
            containers:
                - name: slave-redis-devops
                  image: gcr.io/google_samples/gb-redisslave:v3
                  ports:
                    - containerPort: 6379
                  env:
                    - name: GET_HOSTS_FROM
                      value: "dns"
                  resources:
                    requests:
                        memory: "100Mi"
                        cpu: "100m"
```
> Save and exit the file.
> Apply the deployment using the command:
``` bash
kubectl apply -f redis-slave-deployment.yaml
```
---
4- Create service for redis slave
> Create file `redis-slave-service.yaml` using `vi redis-slave-service.yaml` and add the following content:
``` yaml
apiVersion: v1
kind: Service
metadata:
  name: redis-slave
spec:
    selector:
        app: redis-s
    ports:
    - protocol: TCP
      port: 6379
      targetPort: 6379
```
> Save and exit the file.
> Apply the service using the command:
``` bash
kubectl apply -f redis-slave-service.yaml
```
---
5- Create Frontend deployment
> Create file `frontend-deployment.yaml` using `vi frontend-deployment.yaml` and add the following content:
``` yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  labels:
    app: frontend
spec:
    replicas: 3
    selector:
        matchLabels:
            app: frontend
    template:
        metadata:
            labels:
                app: frontend
        spec:
            containers:
                - name: php-redis-devops
                  image: gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff
                  ports:
                    - containerPort: 80
                  env:
                    - name: GET_HOSTS_FROM
                      value: "dns"
                  resources:
                    requests:
                        memory: "100Mi"
                        cpu: "100m"
```
> Save and exit the file.
> Apply the deployment using the command:
``` bash
kubectl apply -f frontend-deployment.yaml
```
---
6- Create service for frontend
> Create file `frontend-service.yaml` using `vi frontend-service.yaml` and add the following content:
``` yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
    selector:
        app: frontend
    ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30009
    type: NodePort
```
> Save and exit the file.
> Apply the service using the command:
``` bash
kubectl apply -f frontend-service.yaml
```
---
7- Verify the setup
> Get the list of all pods using the command:
``` bash
kubectl get pods
```
> Get the list of all services using the command:
``` bash
kubectl get services
```
