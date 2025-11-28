## Create a PersistentVolume, PersistentVolumeClaim, Pod, and Service in Kubernetes

>Create `pv-xfusion.yaml` file use `vi pv-xfusion.yaml`
``` yaml
kind: PersistentVolume
apiVersion: v1
metadata:
    name: pv-xfusion
spec:
    capacity:
        storage: 3Gi
    accessModes:
        - ReadWriteOnce
    hostPath:
        path: /mnt/sysops
    storageClassName: manual
```
> save and exit the file.
>
> Apply the PersistentVolume using the command:
``` bash
kubectl apply -f pv-xfusion.yaml
```
---
> Create `pvc-xfusion.yaml` file use `vi pvc-xfusion.yaml`
``` yaml
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
    name: pvc-xfusion
spec:
    accessModes:
        - ReadWriteOnce
    resources:
        requests:
            storage: 1Gi
    storageClassName: manual
```
> save and exit the file.
>
> Apply the PersistentVolumeClaim using the command:
``` bash
kubectl apply -f pvc-xfusion.yaml
```
---
>Create `pod-xfusion.yaml` file use `vi pod-xfusion.yaml`
``` yaml
kind: Pod
apiVersion: v1
metadata:
    name: pod-xfusion
    labels:
        app: pod-xfusion
spec:
    volumes:
        - name: storage-xfusion
          persistentVolumeClaim:
              claimName: pvc-xfusion
    containers:
        - name: container-xfusion
          image: httpd:latest
          volumeMounts:
            - name: storage-xfusion
              mountPath: /usr/local/apache2/htdocs/
          ports:
            - containerPort: 80
```
> save and exit the file.
>
> Apply the Pod using the command:
``` bash
kubectl apply -f pod-xfusion.yaml
```
---
> Create `service-xfusion.yaml` file by `vi service-xfusion.yaml`
``` yaml
kind: Service
apiVersion: v1
metadata:
    name: web-xfusion
spec:
    selector:
        app: pod-xfusion
    type: NodePort
    ports:
        - protocol: TCP
          port: 80
          targetPort: 80
          nodePort: 30008
```
> save and exit the file.
>
> Apply the Service using the command:
``` bash
kubectl apply -f service-xfusion.yaml
```
---