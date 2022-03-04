

#QUESTION

Last week the Nautilus DevOps team met with the application development team and decided to containerize several of their applications. The DevOps team wants to do some testing per the following:

Install docker-ce and docker-compose packages on App Server 1.

Start docker service.


#SOLUTION

#Login to the App server and switch to root user 

    ssh user@servername

#Install Docker Compose
    
    curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)"  -o /usr/local/bin/docker-compose
    
    mv /usr/local/bin/docker-compose /usr/bin/docker-compose
    
    chmod +x /usr/bin/docker-compose

#Verify the version 
    
    docker-compose --version

#Install docker-ce 
    
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

    yum install docker-ce docker-ce-cli containerd.io 

#Start the service
    
    systemctl enable docker 

    systemctl start docker

    systemctl status docker 

#Verify the installation 
    
    rpm -qa | grep docker 
    
    docker-compose --version
    
    docker --version








#References
https://stackoverflow.com/questions/36685980/why-is-docker-installed-but-not-docker-compose
https://www.youtube.com/watch?v=akUI6iSroiI 
https://docs.docker.com/engine/install/ubuntu/ 
https://docs.docker.com/engine/install/ 
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04


