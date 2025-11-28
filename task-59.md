## Update the Redis Deployment to use the correct image and ConfigMap
> Use the command to edit the Redis Deployment:
``` bash
kubectl edit deployment redis-deployment
```

> Missing `e` in the image name `redis: alpin` > `redis:alpine`
---
> Missing `f` in Configmap name `redis-conig` > `redis-config`

> save and exit the editor. The Deployment will be updated automatically.