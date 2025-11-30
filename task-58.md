## Day 58: Deploy Grafana on Kubernetes Cluster

1- Create Grafana Deployment
``` yaml
``` bash
kubectl create deployment grafana-deployment-xfusion --image=grafana/grafana --port=3000 
```
---
2- Create Service to expose Grafana Deployment
> Create file `grafana-service-xfusion.yaml` using `vi grafana-service-xfusion.yaml` and add the following content:
``` yaml
kind: Service
apiVersion: v1
metadata:
    name: grafana-service-xfusion
spec:
    type: NodePort
    selector:
        app: grafana-deployment-xfusion
    ports:
        - protocol: TCP
          port: 3000
          targetPort: 3000
          nodePort: 32000
```
> save and exit the file.
> 
> Apply the Service using the command: 
``` bash
kubectl apply -f grafana-service-xfusion.yaml 
```
---
3- Verify the Deployment and Service
> Verify the Grafana Deployment using the command:
``` bash
kubectl get deployments
kubectl get services
```