## Day 66: Deploy MySQL on Kubernetes

1- Create PersistentVolume and PersistentVolumeClaim

> Create file `mysql-pv-pvc-secret.yaml` using `vi mysql-pv-pvc-secret.yaml` and add the following content:
``` yaml
kind: PersistentVolume
apiVersion: v1
metadata:
    name: mysql-pv
spec:
    capacity:
        storage: 250Mi
    accessModes: [ReadWriteOnce]
    hostPath:
        path: "/tmp/data"
---
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
    name: mysql-pv-claim
spec:
    accessModes: [ReadWriteOnce]
    resources:
        requests:
            storage: 250Mi
```

> Save and exit the file.

> Apply the PersistentVolume and PersistentVolumeClaim using the command:
``` bash
kubectl apply -f mysql-pv-pvc-secret.yaml
```
---
2- Create Secrets

> Create file `mysql-secrets.yaml` using `vi mysql-secrets.yaml` and add the following content:
``` yaml
kind: Secret
apiVersion: v1
metadata:
    name: mysql-root-pass
type: Opaque
stringData:
    password: YUIidhb667
---
kind: Secret
apiVersion: v1
metadata:
    name: mysql-user-pass
type: Opaque
stringData:
    username: kodekloud_sam
    password: LQfKeWWxWD
---
kind: Secret
apiVersion: v1
metadata:
    name: mysql-db-url
type: Opaque
stringData:
    database: kodekloud_db2
```
> Save and exit the file.
>
> Apply the Secrets using the command:
``` bash
kubectl apply -f mysql-secrets.yaml
```
---
3- Create MySQL Deployment
> Create file `mysql-deployment-service.yaml` using `vi mysql-deployment-service.yaml` and add the following content:

``` yaml
kind: Deployment
apiVersion: apps/v1
metadata:
    name: mysql-deployment
    labels:
        app: mysql
spec:
    replicas: 1
    selector:
        matchLabels:
            app: mysql
    template:
        metadata:
            labels:
                app: mysql
        spec:
            containers:
                - name: mysql-container
                  image: mysql:5.7
                  env:
                    - name: MYSQL_ROOT_PASSWORD
                      valueFrom: 
                        secretKeyRef:
                            name: mysql-root-pass
                            key: password
                    - name: MYSQL_DATABASE
                      valueFrom: 
                        secretKeyRef:
                            name: mysql-db-url
                            key: database
                    - name: MYSQL_USER
                      valueFrom: 
                        secretKeyRef:
                            name: mysql-user-pass
                            key: username
                    - name: MYSQL_PASSWORD
                      valueFrom: 
                        secretKeyRef:
                            name: mysql-user-pass
                            key: password
                  volumeMounts:
                    - name: mysql-data
                      mountPath: /var/lib/mysql
            volumes:
                - name: mysql-data
                  persistentVolumeClaim:
                    claimName: mysql-pv-claim
```
> Save and exit the file.
>
> Apply the MySQL Deployment using the command:
``` bash
kubectl apply -f mysql-deployment-service.yaml
```
---
4- Create MySQL Service
> Create file `mysql-service.yaml` using `vi mysql-service.yaml` and add the following content:
``` yaml
kind: Service
apiVersion: v1
metadata:
    name: mysql
spec:
    type: NodePort
    selector:
        app: mysql
    ports:
        - protocol: TCP
          port: 3306
          targetPort: 3306
          nodePort: 30007
```
> Save and exit the file.
>
> Apply the MySQL Service using the command:
``` bash
kubectl apply -f mysql-service.yaml
```
---
5- Verify the Deployment and Service
> Verify the MySQL Deployment using the command:
``` bash
kubectl get deployments
```
> Verify the MySQL Service using the command:
``` bash
kubectl get services
```
---