# J.Gawat-Repsoitory
Repository for Cybersecurity UofT - Jonathan Gawat
# Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://drive.google.com/file/d/1r2ZL1f2hSGOthZL0ABX-Stnnk5zN3KC4/view?usp=sharing

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the docker file may be used to install only certain pieces of it, such as Filebeat.

  - _Enter the playbook file._
/etc/ansible# install-elk.yml - for configuring the Elk VM with docker
https://docs.google.com/document/d/17U_eoQ1oIVQUfedsKf1zBYeZJgWbObb3c1xlGJyLcME/edit?usp=sharing


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
-: What aspect of security do load balancers protect? What is the advantage of a jump box?_

Load balancers provides an access point for services used by multiple machines. They also contain their own firewall capabilities serving as increased security in addition to firewalls provided at the original point of access.
A jump box is advantageous because it serves as an access point (or jump-into point) into a remote network. Due to the added features of having a jump-box includes access to private networks using more security measures like an SSH key and port allowing/disabling.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files and system resources.
-: What does Filebeat watch for?_
Filebeat watches for system logs and changes to Elasticbeat 

-: What does Metricbeat record?_
Metricbeat is used for gathering metrics and system resources in Elasticsearch


The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.7   | Ubuntu           |
| Web-1    | Web VM   | 10.0.0.8   | Ubuntu           |
| Web-2    | Web VM   | 10.0.0.9   | Ubuntu           |
| Project-VM ELK|     | 10.1.0.4   | Ubuntu           |                  

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box/ansible machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
-: Add whitelisted IP addresses_
76.71.129.121


Machines within the network can only be accessed by the jumpbox.
-: Which machine did you allow to access your ELK VM? What was its IP address?
Jumpbox IP: 
20.211.7.106 - public IP
10.0.0.7 - private IP

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | SSH port 22 - Yes   | 76.71.129.121        |
| Web-1,2, | No                  |                      |
| Red-Team-LB Port 80|           | Front-end IP 20.70.172.186                     |
| Project-VM ELK Kibana - 5601|  | Front-end IP 20.70.172.186


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
-: What is the main advantage of automating configuration with Ansible?_
It saves time, reduces config errors and full automation of specific servers.

The playbook implements the following tasks:
-: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- Install Docker - installs docker to the Virtual Networks
- Install Python3_pip: allows additional modules to be installed
- Docker Module: Tells PIP module to install docker components
- Increase Memory: Tells the system to use more memory 
- Download and Launch ELK Container: Downloads ELK docker and launches it with ports specified 

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)
https://drive.google.com/file/d/1oDsdaKpCoSCyj_hrzcHe5mqa_AKCudr-/view?usp=sharing


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
-: List the IP addresses of the machines you are monitoring_
IP #1: 10.0.0.8 - Web-1
IP #2: 10.0.0.9 - Web-2
IP #3: 10.1.0.4 - Project-VM ELK


We have installed the following Beats on these machines:
-: Specify which Beats you successfully installed_
Filebeat and Metricbeat on:
#1 Web-1: 10.0.0.8
#2: Web-2: 10.0.0.9
#3: Project-VM: 10.1.0.4

These Beats allow us to collect the following information from each machine:
-: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

- Filebeat collects login events to view current users logged on or who is actively logging in and out.
- Metricbeat collects information such as memory and CPU usage to monitor systems to see if there are any processes using system resources 


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install-elk.yml file to /etc/ansible/roles/install-elk.yml .
- Update the hosts file to include ELK 10.1.0.4 
- Run the playbook, and navigate to http://10.1.0.4:5601/app/kibana to check that the installation worked as expected.

_: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
install-elk.yml is the playbook 
Copied to /etc/ansible/roles/

- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_

Within the /etc/ansible/ directory nano hosts file and create [elk] uncommented 10.1.0.4 ansible_python_interpreter=/usr/bin/python3
When installing Filebeat, you specify hosts as [webservers] where as the ELK server is under a specific node [ELK] 

- _Which URL do you navigate to in order to check that the ELK server is running?
http://your_elk_ip:5601/app/kibana
In this case: http://10.1.0.4:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

My Script/YML file to download and run Filebeat
---
- name: installing and launching filebeat
  hosts: webservers
  become: yes
  tasks:

  - name: download filebeat deb
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

  - name: install filebeat deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

  - name: drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

  - name: enable and configure system module
    command: filebeat modules enable system

  - name: setup filebeat
    command: filebeat setup

  - name: start filebeat service
    command: service filebeat start

  - name: enable service filebeat on boot
    systemd:
      name: filebeat
      enabled: yes

My Script for downloading and running Metricbeat

---
- name: installing and launching metricbeat
  hosts: webservers
  become: yes
  tasks:

  - name: download metricbeat deb
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.6.1-amd64.deb

  - name: install metricbeat deb
    command: dpkg -i metricbeat-7.6.1-amd64.deb

  - name: drop in metricbeat.yml
    copy:
      src: /etc/ansible/files/metricbeat-config.yml
      dest: /etc/metricbeat/metricbeat.yml

  - name: enable and configure system module
    command: metricbeat modules enable docker

  - name: setup metricbeat
    command: metricbeat setup

  - name: start metricbeat service
    command: service metricbeat start

  - name: enable service filebeat on boot
    systemd:
      name: metricbeat
      enabled: yes
