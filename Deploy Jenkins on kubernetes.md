#QUESTION

The Nautilus DevOps team is planning to set up a Jenkins CI server to create/manage some deployment pipelines for some of the projects. They want to set up the Jenkins server on Kubernetes cluster. Below you can find more details about the task:

1) Create namespace jenkins

2) Create a Service for jenkins deployment. Service name should be jenkins-service under jenkins namespace, type should be NodePort, targetport should be 8080 and nodePort should be 30008

3) Create a Jenkins Deployment under jenkins namespace, It should be name as jenkins-deployment , labels app should be jenkins , container name should be jenkins-container , use jenkins image, containerPort should be 8080 and replicas count should be 1.



#SOLUTION

1. Check the namesapce and services 

    ```
        kubectl get namesapce

        kubectl get service

    ```
2. Create the namespace "jenkins"

    ```
    kubectl create namesapce jenkins
    ```
    

3. Verify the newly created namesapce

    ```
    kubectl get namesapce
    ```

4. Create yaml file for the jenkins and paste the yaml file. The content of the file is below the references 

      ```
      vi jenkins.yaml
      ```

5. Create the jenkins pod 
    
    ```
    kubectl create -f jenkins.yaml
    ```

6. Wait for deployment and pods to get running status
```
    kubectl get deploy -n jenkins
    kubectl get pods -n jenkins
    kubectl get service -n jenkins

```

7. Validate the task by using curl command 

  ```
      kubectl exec <pod-name> -n jenkins -- curl http://localhost:8080

  ```

#References


https://devopscube.com/setup-jenkins-on-kubernetes-cluster/
https://www.magalix.com/blog/create-a-ci/cd-pipeline-with-kubernetes-and-jenkins 
https://phoenixnap.com/kb/how-to-install-jenkins-kubernetes 



#Yaml file 



apiVersion: v1

kind: Service

metadata:

  name: jenkins-service

  namespace: jenkins

spec:

  type: NodePort

  selector:

    app: jenkins

  ports:

    - port: 8080

      targetPort: 8080

      nodePort: 30008

---

apiVersion: apps/v1

kind: Deployment

metadata:

  name: jenkins-deployment

  namespace: jenkins

  labels:

    app: jenkins

spec:

  replicas: 1

  selector:

    matchLabels:

      app: jenkins

  template:

    metadata:

      labels:

        app: jenkins

    spec:

      containers:

        - name: jenkins-container

          image: jenkins/jenkins

          ports:

            - containerPort: 8080

