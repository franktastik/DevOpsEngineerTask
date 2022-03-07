
## QUESTION
A new MySQL server needs to be deployed on Kubernetes cluster. The Nautilus DevOps team was working on to gather the requirements. Recently they were able to finalize the requirements and shared them with the team members to start working on it. Below you can find the details:


1.) Create a PersistentVolume mysql-pv, its capacity should be 250Mi, set other parameters as per your preference.

2.) Create a PersistentVolumeClaim to request this PersistentVolume storage. Name it as mysql-pv-claim and request a 250Mi of storage. Set other parameters as per your preference.

3.) Create a deployment named mysql-deployment, use any mysql image as per your preference. Mount the PersistentVolume at mount path /var/lib/mysql.

4.) Create a NodePort type service named mysql and set nodePort to 30007.

5.) Create a secret named mysql-root-pass having a key pair value, where key is password and its value is YUIidhb667, create another secret named mysql-user-pass having some key pair values, where first key is username and its value is kodekloud_cap, second key is password and value is GyQkFRVNr3, create one more secret named mysql-db-url, key name is database and value is kodekloud_db9

6.) Define some Environment variables within the container:

a) name: MYSQL_ROOT_PASSWORD, should pick value from secretKeyRef name: mysql-root-pass and key: password

b) name: MYSQL_DATABASE, should pick value from secretKeyRef name: mysql-db-url and key: database

c) name: MYSQL_USER, should pick value from secretKeyRef name: mysql-user-pass key key: username

d) name: MYSQL_PASSWORD, should pick value from secretKeyRef name: mysql-user-pass and key: password

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.



#SOLUTION

#Check existing running Pods  & Services. 

    kubectl get all 

    kubectl get po,svc 

#Create a  Secret as mentioned in the task change the username & password. Keyword searched on kubernetes documentation page = 

    kubectl create secret generic <secret-name> --from-literal=<key=value pair>

    kubectl create secret generic <secret name> --from-literal=<key=value pair> --from-literal=<key=value pair>

    kubectl create secret generic <secret-name> --from-literal=<key=value pair> --from-literal=<key=value pair>

    kubectl create secret generic <secret name >--from-literal=<key=value pair> 

or

#Alternatively, you can add a definition file for the secret as i did in my yaml definition file. 

#Validate the created secret
    
    kubectl get secret 

#Create a  YAML  file with all the parameters requested from the Question. Keyword searched on kubernetes documentation page = PersistentVolume, PersistentVolumeClaim, Deployment, MySQL. 
#The yaml contains definition file for PersistentVolume, PersistentVolumeClaim, Service and Deployment


#Create the 

    kubectl create -f <filename.yaml>

#Verify PersistentVolume, PersistentVolumeClaim and the pod

    kubectl get pv, pvc 

    kubectl get all -o wide 

#Wait for the deployment and pods running status 

#Validate pod login and check the printenv. Compare the result of the printenv result with the requirement for the task. 

    kubectl exec -it <pod-name> -- /bin/bash

    printenv


        
### Yaml file is here below 

 ``` 

apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 250Mi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"

---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 250Mi

---
apiVersion: v1
kind: Secret
metadata:
  name: mysql-root-pass
type: kubernetes.io/basic-auth
stringData:
  password: YUIidhb667

---  
apiVersion: v1
kind: Secret
metadata:
  name: mysql-user-pass
type: kubernetes.io/basic-auth
stringData:
  username: kodekloud_cap
  password: GyQkFRVNr3
  
---  
apiVersion: v1
kind: Secret
metadata:
  name: mysql-db-url
type: Opaque
stringData:
  database: kodekloud_db9
  
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
spec:
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - image: mysql:5.6
        name: mysql
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
        ports:
        - containerPort: 3306
          name: mysql
        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-persistent-storage
        persistentVolumeClaim:
          claimName: mysql-pv-claim

---
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  type: NodePort
  ports:
  - port: 3306
    targetPort: 3306
    nodePort: 30007
  selector:
    app: mysql
    
    
 ```


### References

https://linoxide.com/deploy-mysql-on-kubernetes/
https://phoenixnap.com/kb/kubernetes-mysql
https://cloud.netapp.com/blog/how-to-set-up-mysql-kubernetes-deployments 
https://statehub.io/resources/guides/deploying-mysql-on-kubernetes-with-cross-region-or-multicloud-business-continuity-using-statehub/
https://dev.to/musolemasu/deploy-a-mysql-database-server-in-kubernetes-static-dpc 
