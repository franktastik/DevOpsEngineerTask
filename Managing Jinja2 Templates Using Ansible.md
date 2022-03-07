
### QUESTION 

One of the Nautilus DevOps team members is working on to develop a role for httpd installation and configuration. Work is almost completed, however there is a requirement to add a jinja2 template for index.html file. Additionally, the relevant task needs to be added inside the role. The inventory file ~/ansible/inventory is already present on jump host that can be used. Complete the task as per details mentioned below:

a. Update ~/ansible/playbook.yml playbook to run the httpd role on App Server 3.

b. Create a jinja2 template index.html.j2 under /home/thor/ansible/role/httpd/templates/ directory and add a line This file was created using Ansible on <respective server> (for example This file was created using Ansible on stapp01 in case of App Server 1). Also please make sure not to hard code the server name inside the template. Instead, use inventory_hostname variable to fetch the correct value.

c. Add a task inside /home/thor/ansible/role/httpd/tasks/main.yml to copy this template on App Server 3 under /var/www/html/index.html. Also make sure that /var/www/html/index.html file's permissions are 0755.

d. The user/group owner of /var/www/html/index.html file must be respective sudo user of the server (for example tony in case of stapp01).

Note: Validation will try to run the playbook using command ansible-playbook -i inventory playbook.yml so please make sure the playbook works this way without passing any extra arguments.



### SOLUTION

#### Verify the playbook in the folder mentioned. 
    
    
    cd ~/ansible/ or cd /home directory/ansible
    ll or ls 
    
    
#### Edit the existing playbook and add the required host in the directory above.  
    
    vi playbook.yml
    
#### Edit the file below by adding the server in the "hosts: Server_name"
 

    - hosts:stapp03
      become: yes
      become_user: root
      roles:
        - role/httpd

##### Verify the playbook above using the cat command.

#### Check the content of inventory 
    
    cat inventory

#### B. Create a jinja2 template file under the directory 
    
    cd directory path /home/thor/ansible/role/httpd/templates/ and create the file name index.html.j2  
    vi index.html.j2
    cat index.html.j2 //To verify the file content
    

#### Add the file mentioned in question 2 while making sure not to hard code the server name inside the template
    
    This file was created using Ansible on {{ansible_hostname}}

#### C. Navigate to /home/thor/ansible/role/httpd/tasks/main.yml and edit the file main.yml file using vim command. Append the jinja template to generate index.html. Verify the permission.  
    
    cd /home/thor/ansible/role/httpd/tasks
    vi main.yml 

    -   name: Use the jinja2 template to generate index.html
        template:
            src: <source of the jinja2 2 template from "b" + index.html.j2>
            dest: <destination from "c">
            mode: <"file permission from question "C"">
            owner: "{{ansible_user}}" //granting privilege as requested in "d" sudo user of server 
            group: "{{ansible_user}}" //granting privilege as requested in "d" sudo user of server 


#### Execute the playbook from the ansible directory
    
    cd ansible
    ansible-playbook -i inventory playbook.yml


#### Validate the task by running the command below in the ansible directory. It should display "The file was created using ansible on Server_name" 
    
    ansible Server_name -a "cat /var/www/html/index.html" -i inventory

    Should display the added file content of the jinja2 template

#### Reference 
    
    
    https://blog.learncodeonline.in/everything-about-ansible-jinja2-templating 
    https://docs.ansible.com/ansible/latest/user_guide/playbooks_templating.html
    https://www.linuxtechi.com/configure-use-ansible-jinja2-templates/
    https://blog.knoldus.com/deploying-custom-files-with-ansible-jinja2-templates/ 
    https://www.linuxhowto.net/how-to-configure-and-use-ansible-jinja2-template/ 
    https://docs.ansible.com/ansible/latest/user_guide/become.html 
