The Nautilus DevOps team is planning to test several Ansible playbook on different app server in Stratos DC. Before that, some pre-requisite must be met.
Essentially, the team needs to set up a password-less SSH connection between Ansible controller and Ansible managed nodes. One of the tickets is assigned to you;
Please complete the task as per details mentioned bellow:

a. Jump host is our Ansible controller, and we are going to run Ansible playbooks through thor user on jump host.

b.Make appropriate changes on jump host so that user thor on jump host can SSH into App Server 1 through its respective sudo user. (for example tony for app server 1).

c. There is an inventory file /home/thor/ansible/inventory on jump host. Using that inventory file test Ansible ping from jump host to App Server 1, make sure ping works.



### SOLUTION

#cd to /home/thor/

#Check the inventory file content

#Generate SSH key 
    
    ssh-keygen

#Copy SSH key to App Server.cd into ~/.ssh and run the command below 

    ssh-copy-id -i ~/.ssh/id_rsa.pub user@remote-host

#Validate the task using the ansible command. cd into the /ansible and run the command below  

    ansible <servername> -m ping -i inventory -v 


#References
https://linuxhint.com/use-ansible-ping-module/ 
https://linuxhint.com/use-ssh-copy-id-command/ 
https://www.simplified.guide/ssh/copy-public-key
