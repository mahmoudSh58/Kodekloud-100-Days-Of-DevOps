## Day 63: Deploy Iron Gallery App on Kubernetes

1- Create Namespace
```bash
kubectl create namespace iron-namespace-nautilus
```
---
2- Create iron gallery Deployment
> Create file `iron-deployments-nautilus.yaml` using `vi iron-deployments-nautilus.yaml` and add the following content:
``` yaml
kind: Deployment
apiVersion: apps/v1
metadata:
    name: iron-gallery-deployment-nautilus
    namespace: iron-namespace-nautilus
    labels:
        run: iron-gallery
    
spec:
    replicas: 1
    selector:
        matchLabels:
            run: iron-gallery
    template:
        metadata:
            labels:
                run: iron-gallery
        spec:
            volumes:
                - name: config
                  emptyDir: {}
                
                - name: images
                  emptyDir: {}
            containers:
                - name: iron-gallery-container-nautilus
                  image: kodekloud/irongallery:2.0
                  volumeMounts:
                    - name: config
                      mountPath: /usr/share/nginx/html/data
                    - name: images
                      mountPath: /usr/share/nginx/html/uploads
                  resources:
                    limits:
                        cpu: "50m"
                        memory: "100Mi"
```
> Save and exit the file.
> 
> Apply the Deployments using the command:
``` bash
kubectl apply -f iron-deployments-nautilus.yaml
```
---
3- Create Database Deployment
> Create file `iron-db-deployment-nautilus.yaml` using `vi iron-db-deployment-nautilus.yaml` and add the following content:
```yaml
kind: Deployment
apiVersion: apps/v1
metadata:
    name: iron-db-deployment-nautilus
    namespace: iron-namespace-nautilus
    labels:
        db: mariadb
    
spec:
    replicas: 1
    selector:
        matchLabels:
            db: mariadb
    template:
        metadata:
            labels:
                db: mariadb
        spec:
            volumes:
                - name: db
                  emptyDir: {}

            containers:
                - name: iron-db-container-nautilus
                  image: kodekloud/irondb:2.0
                  
                env:
                    - name: MYSQL_ROOT_PASSWORD
                      value: "Mmmsmm123?"
                    - name: MYSQL_DATABASE
                      value: "database_blog"
            
            - name: MYSQL_USER
                      value: "mahmoud"
                    - name: MYSQL_PASSWORD
                      value: "Mmmsmm123?123"
                  volumeMounts:
                    - name: db
                      mountPath: /var/lib/mysql
```
> Save and exit the file.
>
> Apply the Deployments using the command:
``` bash
kubectl apply -f iron-db-deployment-nautilus.yaml
```
---
4- Create Services
> Create file `iron-services-nautilus.yaml` using `vi iron-services-nautilus.yaml` and add the following content:
```yaml
---
kind: Service
apiVersion: v1
metadata: 
    name: iron-db-service-nautilus
    namespace: iron-namespace-nautilus
spec:
    selector:
        db: mariadb
    type: ClusterIP
    ports:
     - protocol: TCP
       port: 3306
       targetPort: 3306
---
kind: Service
apiVersion: v1
metadata: 
    name: iron-gallery-service-nautilus
    namespace: iron-namespace-nautilus
spec:
    selector:
        run: iron-gallery
    type: NodePort
    ports:
     - protocol: TCP
       port: 80
       targetPort: 80
       nodePort: 32678 
```
> Save and exit the file.
> 
> Apply the Services using the command:
``` bash
kubectl apply -f iron-services-nautilus.yaml
```
---
5- Verify the Deployments and Services
``` bash
kubectl get deployments -n iron-namespace-nautilus
kubectl get services -n iron-namespace-nautilus
```