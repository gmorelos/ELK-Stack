# ELK-Stack
My first project completed for the University of Denver Cybersecurity Bootcamp. Contains the files necessary to redeploy the ELK Stack with Filebeat and Metric beat as optional additions. 

## Automated ELK Stack Deployment

The files in this repository were used to configure the network linked below.

![Network Diagram](http://github.com/gmorelos/ELK-Stack/blob/main/Diagram/Gloria_Network%20Diagram.png "Network Diagram")

<p align="center">
   <img src="./Diagram/Gloria_Network%20Diagram.png">
  </p>

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above, or, alternatively, select files may be used to install only certain pieces of it. For example, if you choose to run filebeat-playbook.yml and not metricbeat-playbook.yml, then only Filebeat will be installed. 

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


Integrating an ELK server along with Filebeat and Metricbeat allows users to easily monitor the vulnerable VMs for changes to the files and system metrics.

The configuration details of each machine may be found below.

| Name                 | Function           | IP Address | Operating System |
|----------------------|--------------------|------------|------------------|
| Jump-Box-Provisioner | Gateway            | 10.0.0.7   | Linux/Ubuntu     |
| ELK-Server           | Application Server | 10.1.0.4   | Linux/Ubuntu     |
| Web-1                | Web Server         | 10.0.0.8   | Linux/Ubuntu     |
| Web-2                | Web Server         | 10.0.0.9   | Linux/Ubuntu     |


### Access Policies

The machines on the internal network are not exposed to the public Internet.

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the IP address of my personal work station (73.243.48.188).
Machines within the network can only be accessed by the Jump-Box-Provisioner (Pub IP 52.170.60.19/Private IP 10.0.0.7).

A summary of the access policies in place can be found in the table below:


| Name                 | Publicly Accessible | Allowed IP Addresses                 |
|----------------------|---------------------|--------------------------------------|
| Jump-Box-Provisioner | Yes                 | 73.243.48.188 (Personal Workstation) |
| ELK-Server           | Yes                 | 73.243.48.188 (Personal Workstation) |
| Web-1                | No                  | 52.170.60.19 (Jump-Box-Provisioner)  |
| Web-2                | No                  | 52.170.60.19 (Jump-Box-Provisioner)  |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because we can now recreate this setup in minutes using the same YAML files used in this project.

The install-elk.yml playbook implements the following tasks:
- Installs docker.io, the docker release for Ubuntu.
- Installs pip3 and the Python docker module.
- Resizes memory.
- Installs ELK container with image sebp/elk:761.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![docker ps](https://github.com/gmorelos/ELK-Stack/blob/main/Images/sebp%20elk.PNG "docker ps output")

### Target Machines & Beats
This ELK server is configured to monitor the following machines: Web-1 (10.0.0.8) and Web-2 (10.0.0.9)

We have installed the following Beats on these machines: Filebeat, Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat is used to forward and centralize logs and files. Types of logs that Filebeat can collect are audit logs, deprecation logs, gc logs, server logs, and more. For example, Filebeat can collect and store deprecation logs so that we can know what features of a software are no longer being used.
- Metricbeat is used to collect metrics from your system and services, such as CPU, memory, or data usage related to services running on the server. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below:
- Copy the install-elk.yml file to /etc/ansible.
- Update the hosts file to include the internal IP addresses of the machines you will be monitoring. Place the Web-1 and Web-2 IP addresses under the [webservers] header and then create a new [elk] header and place the ELK-Server's internal IP underneath. You must also specify python3 after each IP address listed, so write *ansible_python_interpreter=/usr/bin/python3* after each IP address (e.g. *10.0.0.8 ansible_python_interpreter=/urs/bin/python3*).
- Run the playbook, and navigate to *http://<ELK.VM.External.IP>:5601/app/kibana* to check that the installation worked as expected.
