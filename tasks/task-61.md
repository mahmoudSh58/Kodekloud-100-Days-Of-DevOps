## Day 61: Init Containers in Kubernetes
---
1- Create Deployment Manifest

> Create file `ic-deploy-xfusion.yaml` using `vi ic-deploy-xfusion.yaml` command and add the following content:
``` yaml
kind: Deployment
apiVersion: apps/v1
metadata:
    name: ic-deploy-xfusion
spec:
    replicas: 1
    selector:
        matchLabels:
            app: ic-xfusion
    template:
        metadata:
            labels:
                app: ic-xfusion
        spec:
            initContainers:
            - name: ic-msg-xfusion
              image: debian:latest
              command: ['/bin/bash', '-c' , 'echo Init Done - Welcome to xFusionCorp Industries > /ic/beta']
              volumeMounts:
                - name: ic-volume-xfusion
                  mountPath: /ic
            
            containers:
            - name: ic-main-xfusion
              image: debian:latest
              command: ['/bin/bash', '-c' , 'while true; do cat /ic/beta; sleep 5; done']
              volumeMounts:
                - name: ic-volume-xfusion
                  mountPath: /ic
            
            volumes:
            - name: ic-volume-xfusion
              emptyDir: {}
```
> Save and exit the file.
>
> Apply the Deployment manifest using the command:
``` bash
kubectl apply -f ic-deploy-xfusion.yaml
``` 