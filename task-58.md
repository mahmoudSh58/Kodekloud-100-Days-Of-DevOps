> Create a Grafana deployment 
``` bash
kubectl create deployment grafana-deployment-xfusion --image=grafana/grafana --port=3000 
```
> Ymal file for a NodePort service for Grafana on port 32000 
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

> Apply the service file 
``` bash
kubectl apply -f grafana-service-xfusion.yaml 
```