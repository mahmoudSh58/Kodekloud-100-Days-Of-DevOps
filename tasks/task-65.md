## Day 65: Deploy Redis Deployment on Kubernetes

1- Create ConfigMap
``` bash
kubectl create configmap my-redis-config --from-literal redis-config="maxmemory 2mb"
```
---
2- Create Redis Deployment
> Create file `redis-deployment.yaml` using `vi redis-deployment.yaml` and add the following content:
``` yaml
kind: Deployment
apiVersion: apps/v1
metadata:
    name: redis-deployment
spec:
    replicas: 1
    selector:
        matchLabels:
            app: redis
    template:
        metadata:
            labels:
                app: redis
        spec:
            containers:
                - name: redis-container
                  image: redis:alpine
                  ports:
                    - containerPort: 6379
                  resources:
                    requests:
                        cpu: "1"
                  volumeMounts:
                    - name: data
                      mountPath: /redis-master-data
                    - name: redis-config
                      mountPath: /redis-master
            volumes:
                - name: redis-config
                  configMap:
                    name: my-redis-config
                - name: data
                  emptyDir: {}
```
> Save and exit the file.
> 
> Apply the Redis Deployment using the command:
``` bash
kubectl apply -f redis-deployment.yaml
```
---
3 Verify the Deployment
``` bash
kubectl get deployments
```