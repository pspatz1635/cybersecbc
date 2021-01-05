# cybersecbc
Cyber security boot camp work
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Project 1 diagram.jpg](https://github.com/pspatz1635/cybersecbc/blob/main/Diagrams/Project%201%20diagram.jpg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Ansible file may be used to install only certain pieces of it, such as Filebeat.

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting traffic to the network.

The jump box minimizes how many machines are exposed to the internet and creates a single point from which all other containers can be deployed.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the files and system resources.

The configuration details of each machine may be found below.

|         Name         |     Function    | IP Address |        Operating System       |
|:--------------------:|:---------------:|:----------:|:-----------------------------:|
| Jump-Box-Provisioner | Gateway/Bastion |  10.0.1.4  | Linux Ubuntu Server 18.04 LTS |
|         Web-1        |       DVWA      |  10.0.1.5  | Linux Ubuntu Server 18.04 LTS |
|         Web-2        |       DVWA      |  10.0.1.6  | Linux Ubuntu Server 18.04 LTS |
|         Web-3        |       DVWA      |  10.0.1.7  | Linux Ubuntu Server 18.04 LTS |
|    Pablo-ELKServer   |    ELK Server   |  10.1.0.4  | Linux Ubuntu Server 18.04 LTS |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- My public IP: 76.69.xxx.xxx

Machines within the network can only be accessed by SSH through the Jump-Box, except for the ELK Server which can be accessed from my public IP address.

A summary of the access policies in place can be found in the table below.

|              Name             |       Source      |   Destination   | Action | Publicly Accessible |
|:-----------------------------:|:-----------------:|:---------------:|:------:|:-------------------:|
|           SSH-Access          |   76.69.xxx.xxx   | Virtual Network |  Allow |         Yes         |
|        SSH_From_JumpBox       |      10.0.1.4     | Virtual Network |  Allow |          No         |
|         HTTP_for-DVWA         |   76.69.xxx.xxx   | Virtual Network |  Allow |         Yes         |
|        AllowVnetInBound       |  Virtual Netwwork | Virtual Network |  Allow |          No         |
| AllowAzureLoadBalancerInBound | AzureLoadBalancer |       Any       |  Allow |          No         |
|         DenyAllInBound        |        Any        |       Any       |  Deny  |          no         |
|          elk_port_5601        |   76.69.xxx.xxx   | Virtual Network |  Allow |         Yes         |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it simplifies the process and reduces the chance of human error. Furthermore, it standardizes the deployment so it can be redeployed very quickly if the machine needs to be rebuilt or replaced.

The playbook implements the following tasks:
- Install Docker and Python3 Pip.
- Increases virtual memory.
- Downloads, installs and configures Elk container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.


![Docker ps.jpg](https://github.com/pspatz1635/cybersecbc/blob/main/Images/Docker%20ps.jpg)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 10.0.1.5
- Web-2 10.0.1.6
- Web-3 10.0.1.7

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat
These Beats allow us to collect the following information from each machine:
- Filebeat collects log data and centralizes it, this allows us to see all activity on the DVWA machines.
- Metricbeat collects metrics from the operating system and from services and centralizes it, this allows us to the system resources on the DVWA machines.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the Install-elk.yml file to /etc/ansible.
- Update the hosts file to include the ELK Server and it's IP adress.
- Run the playbook, and navigate to Elk Server's public IP adress, http://(elk ip address):5601, to check that the installation worked as expected.


Specific Commands To Install The Elk Server
- To download the install-elk.yml playbook, navigate to the /etc/ansible folder and run:
  - wget "https://github.com/pspatz1635/cybersecbc/blob/main/Ansible/install-elk.yml"
- Run command: nano hosts
  - Add:

[elk]
10.1.0.4 ansible_python_interpreter=/usr/bin/python3

-IP address should be the private IP of your ELK Server

  - Should appear like the image below:

![hosts.jpg](https://github.com/pspatz1635/cybersecbc/blob/main/Images/hosts.jpg)

- Run command: ansible-playbook install-elk.yml
  - Navigate to Elk Server's public IP adress, http://(elk ip address):5601, to check that the installation worked as expected.


###Beats Playbooks

- To install Beats start by navigating to /etc/ansible and running the nano hosts command.
  - Ensure that your target machines are added to the webserver section as shown in the image above.

- In the ansible folder create 2 files and name them roles and files.

- Copy the filebeat-config.yml file to the files folder.
  - Navigate to /etc/ansible/files and run: wget https://github.com/pspatz1635/cybersecbc/blob/main/Ansible/filebeat-config.yml
- Run command: nano filebeat-config.yml
- Edit line 1106 IP address to be the private IP address of the ELK Server.
  - Use "^_" or "shift + ctrl + -" to jump to the line.

![filebeat1106.jpg](https://github.com/pspatz1635/cybersecbc/blob/main/Images/filebeat1106.jpg)

- Edit line 1806 IP address to be the private IP address of the ELK Server.

![filebeat1806.jpg](https://github.com/pspatz1635/cybersecbc/blob/main/Images/filebeat1806.jpg)

- Copy the filebeat-playbook.yml file to the roles folder.
  - Navigate to /etc/ansible/roles and run: wget https://github.com/pspatz1635/cybersecbc/blob/main/Ansible/filebeat-playbook.yml
- Run command: ansible-playbook filebeat-playbook.yml

- Copy the metricbeat-config.yml file to the files folder.
- Run command: nano metricbeat-config.yml
  - Under the header Kibana, edit the hosts:IP Address to the private IP address of the ELK Server.

![metricbeatkibana.jpg](https://github.com/pspatz1635/cybersecbc/blob/main/Images/metricbeatkibana.jpg)

  - Under the header Outputs, edit the hosts:IP Address to the private IP address of the ELK Server.

![metricbeatoutputs.jpg](https://github.com/pspatz1635/cybersecbc/blob/main/Images/metricbeatoutputs.jpg)

- Copy the metricbeat-playbook.yml file to the roles folder.
  - Navigate to /etc/ansible/roles and run: wget https://github.com/pspatz1635/cybersecbc/blob/main/Ansible/metricbeat-playbook.yml
- Run command: ansible-playbook metricbeat-playbook.yml



