## Create a Grafana Deployment and expose it via NodePort Service

> Create Grafana Deployment using the command:
``` yaml
``` bash
kubectl create deployment grafana-deployment-xfusion --image=grafana/grafana --port=3000 
```
---
> Create file `grafana-service-xfusion.yaml` using `vim grafana-service-xfusion.yaml` and add the following content:
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