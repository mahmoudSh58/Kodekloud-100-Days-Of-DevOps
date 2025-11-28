## Create a pod with two containers sharing a volume
> Create file `shared-pod.yaml` using `vim shared-pod.yaml` and add the following content:
``` yaml
kind: Pod
apiVersion: v1
metadata:
  name: webserver
spec:
  volumes:
    - name: shared-logs
      emptyDir: {}

  containers:
    - name: nginx-container
      image: nginx:latest
      volumeMounts:
        - name: shared-logs
          mountPath: /var/log/nginx
    
    - name: sidecar-container
      image: ubuntu:latest
      command: [ "sh","-c","while true; do cat /var/log/nginx/access.log /var/log/nginx/error.log; sleep 30; done" ]
      volumeMounts:
        - name: shared-logs
          mountPath: /var/log/nginx
```
> save and exit the file.
---
> Create the Pod using the command:
```bash
kubectl apply -f shared-pod.yaml
```