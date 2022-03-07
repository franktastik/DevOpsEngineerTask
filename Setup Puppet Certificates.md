### QUESTION

The Nautilus DevOps team has set up a puppet master and an agent node in Stratos Datacenter. Puppet master is running on jump host itself (also note that Puppet master node is also running as Puppet CA server) and Puppet agent is running on App Server 2. Since it is a fresh set up, the team wants to sign certificates for puppet master as well as puppet agent nodes so that they can proceed with the next steps of set up. You can find more details about the task below:

Puppet server and agent nodes are already have required packages, but you may need to start puppetserver (on master) and puppet service on both nodes.

Assign and sign certificates for both master and agent node.


### SOLUTION

On the Master server = puppet server
### On the jump server as root user. Add alias to resolve puppet master node by adding `puppet` to the jump host line config

    vi /etc/hosts

### verify or Start the puppet server

    systemctl status puppetserver 

### View certificates

    puppetserver ca list --all

### View the host files
    cat /etc/hosts


On the Client App Server = 
### SSh into the App server

### View the hosts file

    vi /etc/hosts

### Copy the puppet master server host alias and paste in the client App Server 

### Ping puppet uising the command below 

    ping puppet

### Start the puppet service

    systemctl status puppet

    systemctl restart puppet

    systemctl status puppet
 


On the Master server = puppet server = jump hosts root user 
### View the certificates from the puppetserver

    puppetserver ca list --all

### Sign the certificates

    puppetserver ca sign --all


On the Client App Server = agent Node. 
### Run the command to generate certificates on the agent. 

    puppet agent -t 


### References

https://puppet.com/docs/puppet/7/ssl_regenerate_certificates.html
https://documentation.wazuh.com/current/deploying-with-puppet/setup-puppet/setup-puppet-certificates.html 
