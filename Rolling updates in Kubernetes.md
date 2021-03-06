### QUESTION: 

We have an application running on Kubernetes cluster using nginx web server. The Nautilus application development team has pushed some of the latest changes and those changes need be deployed. The Nautilus DevOps team has created an image nginx:1.17 with the latest changes.
Perform a rolling update for this application and incorporate nginx:1.17 image. The deployment name is nginx-deployment
Make sure all pods are up and running after the update.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.


### SOLUTION:

# N.B: Searched the k8 documentation page and use the keyword = Rolling Updates nginx
```
  <deployment name> = nginx-deployment
  <container name> = nginx-container. 
```

#Check the deployment information. 
    
    Remember that  = <image name>-deployment 
    
    kubectl get deployment <deployment name> -o yaml

    kubectl get deployment <deployment name> -o yaml | grep nginx

    or 

    kubectl get deployment <deployment name> -o yaml | grep nginx: 

    or 

    kubectl get deployment <deployment name> -o yaml | grep image

    Result = old image was 
        image: nginx:1.16  
    
#To update the file manually, Run this command:
    
    kubectl edit deployment <deployment name>

#Change container image to - nginx:1.17

Or 

#To update the file automatically for time saving, use the command below

    kubectl set image deployment/<deployment name> <container name>=<new image name:version>

#Run the watch command to see live rolling update with zero downtime

    watch kubectl get pods

#Check the status of rollout:

    kubectl rollout status deployment/<deployment name>

#check the image

    kubectl get deployment <deployment name> -o yaml





### References
https://phoenixnap.com/kb/kubernetes-rolling-update 
https://kubernetes.io/docs/tutorials/kubernetes-basics/update/update-intro/
https://www.containiq.com/post/kubernetes-rolling-update-deployment 

### Benefits of Rolling Updates
Rolling updates allow changes to be integrated gradually, offering flexibility and control within an application lifecycle. Some benefits of using rolling updates for Kubernetes clusters include:

-Ensures zero downtime since pod instances of the application are always running even during an upgrade
-Allows developers to examine the effect of the changes in a production environment without affecting user experience
- Does not require additional resources assigned to the cluster, making it a cost-effective deployment strategy
- Avoids the requirement of strenuous manual migration of configuration files, where complex upgrades can be achieved efficiently by making simple changes to a deployment file.
