# Automate Implementation of Monitoring Nodes using Prometheus Grafana Ansible

# Getting Started 
In this project we are using Ansible playbooks to do the configuration in the nodes respectively. Generally in Ansible we can use Adhoc commands or ansible playbooks (.yml file) for the configuration management and automation. 
So, this project involves 2 nodes (which we are monitoring) using a monitor server and a controller to configure everything. Monitoring server is nothing but an ec2 instance/linux server/ any virtual machine hosted. 
So here we are considering total 4 vm's / ec2 / linux server. In this one is a controller where we run our Ansible playbooks, another is the monitoring server where prometheus, grafana are hosted and last 2 are the nodes which are being monitored using the monitoring server. 
The project main agenda is efficiency and time saving. Instead of configuring and installing node exporter in every node to monitor the nodes which involves more time. So, in this busy world we can't invest that much time so we came up with the solutoin of using ansible playbooks for this. 
So, here we wrote different roles in the playbooks which involved configuring node exporter, grafana, prometheus. So instead of manually configuring everything the ansible playbook using ssh after getting a secure shell connection with the nodes it configures and installs node exporter and exports the metrics for monitoring. 

# Implementation 
First login to the controller using root user. The controller can be a linux server/ ec2 instance or any VM. (Putty preferrable)
Install Ansible in the controller using below commands 
sudo apt update 
sudo apt install ansible
NOTE: sudo or yum and may vary according to your instance. Please check the docs to install Ansible if any problem occurs, attaching link below 
https://docs.ansible.com/ansible/2.9/installation_guide/intro_installation.html
After installation check whether it got installed or no using the command below
ansible --version
Now navigate to .ssh for generating an ssh key 
Use cd /root/.ssh/
Now generate a ssh key using the command "ssh-keygen", press Enter until the key got generated. 
Now you should be able to see the generated key using command "ls -ltr"
id_rsa.pub is the file where your key is present
Open this file using cat/vi/vi/nano and copy this file content 
Now open the monitoring node and navigate to /root/.ssh/ using "cd"
You can see "Authorized keys" file in this location. In this authorized keys file paste the copied content at the end of the file. Now give permissiong to the authorized keys file and .ssh using "chmod 600 authorized_keys" and "chmod 700 .ssh" 

Now you should be able to connect to the monitoring node through the controller using the command "ssh@monitoringnodeipaddress" from controller cli. 
Similarly setup the ssh connection between node1 and node2 using the same procedure and try to check using "ssh@IPADD"
Once all the connections are set we are ready to configure using our Ansible to monitor the nodes. 
Use the command "ansible-playbook -i {path/to/inventoryfile/inventory} {path/to/yamlfile/.ymlfile}"

You can cross check by manually logging into the nodes and checking for node exporter or manually logging into monitoring node and checking for grafana/prometheus. 



# THEORY EXPLANATION 
# Ansible 
Ansible is an open-source automation tool that simplifies configuration management, application deployment, task automation, and orchestration. 

# Grafana 
 Grafana serves as the visualization and dashboarding platform, providing an intuitive and customizable interface for monitoring and analyzing data. Dashboards are crafted to offer a real-time, consolidated view of critical metrics, empowering DevOps teams to make informed decisions promptly. The monitoring solution is designed to be scalable, accommodating the growth of the infrastructure. Grafana's flexibility ensures adaptability to diverse infrastructure setups, making it suitable for both small-scale and large-scale deployments.

# Prometheus 
Prometheus acts as the core monitoring and alerting component, responsible for collecting and storing time-series data from various nodes in the infrastructure. Customizable Prometheus queries enable the extraction of relevant metrics, empowering operators to gain insights into resource utilization, system performance, and potential bottlenecks.

# Node Exporter 
Node Exporter collects a variety of metrics related to the host machine's operating system and hardware. This includes information about the CPU, memory, disk, network, and other system-level resources. Node Exporter is designed to work seamlessly with Prometheus, a popular open-source monitoring and alerting toolkit. Prometheus scrapes (pulls) metrics from Node Exporter endpoints at regular intervals. Node Exporter exposes metrics through HTTP endpoints, typically on port 9100 by default. Prometheus scrapes these endpoints to collect the metrics. The exposed metrics follow the Prometheus exposition format.

# About Ansible playbooks and adhoc commands
Basically, Ansible adhoc commands are used when you want to execute small commands (like "ls", etc) but when you want to execute bigger commands for configuration management or deploymet or automation it is suggested to use Ansible playbooks. 
Since our project deals with larger commands we have used Ansible playbooks (.yml file)
Ansible (.yml file ) explanation. 
We have used Ansible roles to keep our ansible in a structured way 
In Ansible, roles are a way to organize and structure your playbooks by breaking them into smaller, reusable components. In a simpler way a role is to organize playbooks and reuse them.  Here we have 3 different things to do 1 is installing and configuring grafana (which is our role 1) 2 is configuring node exporter in our nodes (role 2 ) and 3 is configuring prometheus to scrape metrics using node exporter  (role 3). 
So, we have created 3 roles in roles folder

# Ansible file structure explanation
Deep dive into Ansible --> Roles  -- > File structure 
Files - The files/ directory contains static files that need to be transferred to the managed nodes during the playbook execution. These can include configuration files, scripts, or any other files required by the tasks.
Tasks - The tasks/ directory is where the main work of a role happens. It contains YAML files defining tasks that Ansible should perform on the managed nodes. Tasks can include actions like installing packages, configuring services, or copying files.
Handlers - The handlers/ directory contains Ansible handlers. Handlers are special tasks that are only run if a task notifies them. They are typically used to restart services or perform other actions in response to changes in the system.
Vars - The vars/ directory holds variable files. These files can define variables that are used in the tasks. Variables allow you to make your roles more flexible and reusable by parameterizing values that might change based on the environment or specific use case.
Templates - The templates/ directory is used for Jinja2 templates. Templates are text files with placeholders for variables and expressions. They are processed by Ansible and can be used to dynamically generate configuration files or scripts based on the values of variables.

# Ansible inventory file explained 
[name_of_the_gropu]

 ip address of the instance you want to configure or install the requirements / host address.


