## Day 64: Fix Python App Deployed on Kubernetes Cluster

> Edit the deployment
```bash
kubectl edit deployment python-deployment-datacenter
```
change the image to `poroko/flask-demo-app` and save the file.

---
> Edit the service
``` bash
kubectl edit service python-service-datacenter
```

change the port and targetPort to `5000` and save the file.