## Automated ELK Stack Deployment


The files in this repository were used to configure the network depicted below.


[Diagrams/ElkStackDeployment.png]


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the docker_install_playbook.yml file may be used to install only certain pieces of it, such as Filebeat.


* [docker_install_playbook.yml] - used to install DVWA servers
* [install-elk.yml] - used to install ELK server
* [filebeat-playbook.yml] - used to install and configure Filebeat on ELK Server and DVWA Server
* [LuckyDuck.sh] - used to find the most losses on table


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build




### Description of the Topology


The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.


Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
Load balancers protect the servers from being overloaded with traffic. The advantage of having a JumpBox is having one VM with administrative access to a virtual network so that it isn't available to just anyone.


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs  and system traffic.
- Filebeat watches and collects events and log files.
- Metricbeat records statistical data from the OS and services running on the servers.


The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.


+----------+-----------+------------+------------------+
| Web      | Function  | IP Address | Operating system |
+----------+-----------+------------+------------------+
| Jump-Box | GateWay   | 10.1.0.4   | Linux            |
+----------+-----------+------------+------------------+
| Web-1    | DVWA Serv | 10.1.0.7   | Linux            |
+----------+-----------+------------+------------------+
| Web-2    | DVWA Serv | 10.1.0.6   | Linux            |
+----------+-----------+------------+------------------+
| Web-3    | DVWA Serv | 10.1.0.8   | Linux            |
+----------+-----------+------------+------------------+
| Elk Serv | ELK Stack | 10.0.0.4   | Linux            |
+----------+-----------+------------+------------------+




The machines on the internal network are not exposed to the public Internet. 


Only the JumpBox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP address: 68.9.193.14




Machines within the network can only be accessed by SSH via JumpBox.
-Only my JumpBox has access to my ELK and the IP address is 10.1.0.4


A summary of the access policies in place can be found in the table below.


| Name     | Publicly Accessible | Allowed IP Addresses   |
|----------|---------------------|------------------------|
| Jump-Box | Yes                 | SSH by my local IP     |
| Web-1    | No                  | 20.29.122.141 10.1.0.4 |
| Web-2    | No                  | 20.29.122.141 10.1.0.4 |
| Web-3    | No                  | 20.29.122.141 10.1.0.4 |
| Elk Serv | No                  | SSH 10.0.0.4:5601      |




### Elk Configuration


Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
-Automating the deployment sends it to multiple machines simultaneously, eliminating the need of running multiple scripts on each machine


The playbook implements the following tasks:
* Installs Docker.io
* Installs pip3
* Change VM Memory size
* Download and configure ELK docker Container
* Set Ports
The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.


[Diagram/docker_ps.png]
  



### Target Machines & Beats
This ELK server is configured to monitor the following machines:
-Web-1 (10.1.0.7) Web-2 (10.1.0.6) Web-3 (10.1.0.8)


We have installed the following Beats on these machines:
- Filebeats


These Beats allow us to collect the following information from each machine:
- Filebeats collects and forwards log data from the machine it's installed on and sends the information to the ELK stack server for processing. The user can then view this information thru Kibana via charts and tables.


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 


SSH into the control node and follow the steps below:
- Copy the filebeatconfig.yml file to /etc/ansible
- Update the /etc/ansible/host file to include the IP addresses of DVWA’s
- Run the playbook, and navigate to 20.70.197.121:5601/app/kibana to check that the installation worked as expected.


_
- The playbook is install-elk.yml and is copied to /etc/ansible
- Update the file /etc/ansible/hosts.cg then add the ip addresses of the machine you want the playbooks installed on.
- Navigate to 20.70.197.121:5601/app/kibana to make sure the ELK Server is up and running. (above is the Elk Server’s public IP address)


_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._


SSH to JumpBox from local machine
* ssh azadmin@10.1.0.4


View list of docker containers:
* sudo su
* docker container list -a


 Start up container:
* sudo docker start youthful_roentgen


Attach to container:
* docker attach youthful_roentgen


Add DVWA’s to host files: 
* Nano /etc/ansible/hosts
* Add additional private IP’s under [webservers]


Copy filebeat configuration file:
* Copy filbeatconfig.yml to /etc/ansible


Run ansible playbook for filebeat configuration
* ansible-playbook /etc/ansible/filebeatplaybook.yml


Open Kibana
* 20.70.197.121:5601/app/kibana