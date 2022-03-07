### QUESTION

One of the DevOps engineers was trying to deploy a python app on Kubernetes cluster. Unfortunately, due to some mis-configuration, the application is not coming up. Please take a look into it and fix the issues. Application should be accessible on the specified nodePort.

The deployment name is python-deployment-nautilus, its using poroko/flask-demo-appimage. The deployment and service of this app is already deployed.

nodePort should be 32345 and targetPort should be python flask app's default port.
Note: The kubectl on jump_host has been configured to work with the kubernetes cluster.

Result from the applications url shows 
    502 Bad Gateway 
    nginx/1.21.1

#SOLUTION

#Check the existing pods, service and deployment

    kubectl get pods, svc
    kubectl get deploy
    

#Check the logs for error messages. The error messages gives an insight on what to fix. 

    kubectl logs <pod-name>


#Use the command below and read the event logs 

    kubectl describe pod | grep Events -A10 

#The error message is showing that the    

    Error from server (BadRequest): container "python-container-xfusion" in pod "python-deployment-xfusion-66fbc54cf5-q5bb8" is waiting to start: trying and failing     to pull image

#Edit the image name in the Deployment definition file and save the file using the :wq 

    kubectl edit deployment

#Wait for the pod and deployment status = running

    kubectl get deployment 

#View the target port set in the Service definition file. 

    kubectl get svc

#Based on the question, set the targetPort to Python flask app default port which is port 5000. 
    
    kubectl edit svc 
    
#Search for port 8080 using / in the vim editor. Change the port 8080 to the default python flask app default port = 5000. Replace  all 8080 port by 5000 port

#View the new port configuration

    kubectl get svc
    


    
