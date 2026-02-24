## Day 62: Manage Secrets in Kubernetes

1- Create Secret from File
``` bash
kubectl create secret generic blog --from-file=blog.txt=/opt/blog.txt
```
---
2- Create Pod and Mount the Secret

> Create `secret-pod-datacenter.yaml` file using `vi secret-pod-datacenter.yaml` and add the following content:
``` yaml
kind: Pod
apiVersion: v1
metadata:
    name: secret-datacenter
spec:
    volumes:
        - name: secret-volume
          secret:
            secretName: blog
    containers:
        - name: secret-container-datacenter
          image: fedora:latest
          command: ['sh','-c','sleep 10000']
          volumeMounts:
            - name: secret-volume
              mountPath: /opt/cluster

```
> save and exit the file.
>
> Apply the Pod using the command:
``` bash
kubectl apply -f secret-pod-datacenter.yaml
```
---
3- Verify the Secret in the Pod

``` bash
kubectl exec -it secret-datacenter -- ls /opt/cluster
```
