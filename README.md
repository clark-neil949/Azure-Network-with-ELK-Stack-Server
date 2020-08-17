## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Topology](https://github.com/clark-neil949/Azure-Network-with-ELK-Stack-Server/blob/master/Images/Azure_Network_Diagram_with_ELK_Stack.PNG)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _yaml_ file may be used to install only certain pieces of it, such as Filebeat.

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
      src: /etc/ansible/files/filebeat-configuration.yml
      dest: /etc/filebeat/filebeat.yml

  - name: enable and configure system module
    command: filebeat modules enable system

  - name: setup filebeat
    command: filebeat setup

  - name: start filebeat service
    command: systemctl start filebeat

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly _available_, in addition to restricting permissions to the _network_.
- What aspect of security do load balancers protect? _To protect an application from losings service when one server goes down another availabilty zone is still active. To direct traffic evenly or redirect to available zone._

- What is the advantage of a jump box? _By using jumpbox you can restrict which IP addresses have access. We can also monitor who logs in and out._

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _virtual machines_ and system _logs_.
- What does Filebeat watch for?
_Monitors and Collect log files._
- What does Metricbeat record?
_System wide and CPU and memory statistics for an elasticsearch deployment._

The configuration details of each machine may be found below.

| Name     | Function        | IP Address | Operating System |
|----------|-----------------|------------|------------------|
| Jump Box | Gateway         | 10.0.0.4   | Linux            |
| Web-1    | Web Server      | 10.0.0.5   | Linux            |
| Web-2    | Web Server      | 10.0.0.6   | Linux            |
| ELK      | ELK Stack Server| 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the _local workstation_ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _50.26.190.41_

Machines within the network can only be accessed by _jumpbox_.
- _Which machine did you allow to access your ELK VM? Jumpbox and two web servers._
- _What was its IP address? 10.0.10.4, 10.0.0.5, 10.0.0.6_

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 10.0.0.5 10.0.0.6    |
| Web-1    | No                  |                      |
| Web-2    | No                  |                      |
| ELK      | No                  |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _The main advantage of automating configuration with Ansible is that it can speed up the deployment of a virtual network._

The playbook implements the following tasks:
- _Install docker.io_
- _Install python-pip_
- _Install docker module pip_
- _Increase virtual memory_
- _Downlaod and launch a docker elk container_

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![docker_ps.png](https://github.com/clark-neil949/Azure-Network-with-ELK-Stack-Server/blob/master/Images/docker_ps.PNG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _10.0.0.5 (Web-1)_
- _10.0.0.6 (Web-2)_

We have installed the following Beats on these machines:
- _Web-1: Filebeat and Metricbeat_
- _Web-2: Filebeat and Metricbeat_

These Beats allow us to collect the following information from each machine:
- _This data collected using this beat can be from the syslog which can tell us what proccess started running and the time the process started. The data can also show you what commands have been run as the root user._
### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _yaml_ file to _host VM_.
- Update the _yaml_ file to include...
- Run the playbook, and navigate to _host VM_ to check that the installation worked as expected.

You should see the following screen on Kibana under Module Status, Check Data, Verify Incoming Data if the Filebeat installation was successful:

![Filebeat data recieved](https://github.com/clark-neil949/Azure-Network-with-ELK-Stack-Server/blob/master/Images/Day2DataRecievedFromModule.PNG)

![Filebeat screen](https://github.com/clark-neil949/Azure-Network-with-ELK-Stack-Server/blob/master/Images/Day2Filebeat.PNG)

You should see the following screen on Kibana under Docker Metrics, Module Status, Check Data, Verify Incoming Data if the Metricbeat installation was successful:

![Metricbeat screen](https://github.com/clark-neil949/Azure-Network-with-ELK-Stack-Server/blob/master/Images/Metricbeat.png)

_Answer the following questions to fill in the blanks:_
- Which file is the playbook? Where do you copy it? _The yaml(.yml) file is the playbook. You copy it onto the host VM._
- Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on? _You must update the hosts file. In the heading of the yaml file next to host, specifify which machine you want to perform the install using the category you listed the machine on in the hosts file._
- Which URL do you navigate to in order to check that the ELK server is running? _http://40.76.11.186:5601_

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

- _ssh azureuser@(IpAddress_of_your_JumpBox)_
- _docker run -ti container/ansible_
- _change dir. /etc/ansible_
- _ssh-keygen to your web service_
- _nano hosts (update IP on[webservers][ELK server])_
- _nano ansible.cfg (remote_user to which server you want to use)_
