# Project-1
Cybersecurity cloud security

## Automated ELK Stack Deployment

The files in this repository have been created and tested to generate a live ELK stack deployment in Azure. The files can be used to recreate the deployment in the accompanying diagram.

### Description of the Topology

This network is configured to run  a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will have high availablity. It also aids in restricting access to the network.
- The load balancers help protect the servers from a denial of service attack. The load balancer distributes traffic amongst the backend servers.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files and system performance.
- The log files are monitored by Filebeat


The configuration details of each machine may be found below.

| Name                  | Function   | IP Address | Operating System |
|-----------------------|------------|------------|------------------|
| Jump-Box-Provisioner  | Gateway    | 10.0.0.4   |Linux ubuntu 18.04|
| Web-1                 | VM         | 10.0.0.9   |Linux ubuntu 18.04|
| Web-2                 | VM         | 10.0.0.10  |Linux ubuntu 18.04|
| Web-3                 | VM         | 10.0.0.11  |Linux ubuntu 18.04|
| ELK VM                | ElkStack   | 10.1.0.4   |Linux ubuntu 18.04|

### Access Policies

The VM's comprising the internal network are not exposed to the public. 

Only the jump box provisioner can accept outside connections. Access to this machine is only allowed from the following IP address:
- 47.233.13.203

Machines within the network can only be accessed by Port 22.
- 10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box |     No              | 40.83.216.225        |
| DVWA-VMs |     No              | 10.0.0.4             |
| ELK Stack|     No              | 10.0.0.4             |

### Elk Configuration

Ansible is used to automate the configuration of the ELK stack. No configuration was performed manually.

The playbook implements the following tasks:
* Change to the memory setting on the ELK VM
* Install docker.io
* Install python-pip
* Install docker python module
* Download and launch a docker elk stack

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
* 10.0.0.9
* 10.0.0.10
* 10.0.0.11

The following beat was installed on these machines:
- filebeat-7.6.1-amd64.deb

Filebeat allow us to collect the following information from each machine:
-  Filebeat monitors and collects log events on specificed servers. It is being used in this configuration to collect and send log files to kibana.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the Filebeat-configuration.yml file to the ELK VM.
- Update the hosts file to include webservers 10.0.1.7, 10.0.1.8, 10.0.1.9, 10.0.1.10
- Run the playbook, and navigate to Kibana to check that the installation worked as expected.

`$ ansible-playbook filebeat-playbook.yml`

```
---
  - name: filebeat installer
    hosts: webservers
    become: true
    tasks:
    
    - name: download filebeat
      shell: curl -L -O  https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb
            
    - name: install filebeat
      shell: dpkg -i filebeat-7.6.1-amd64.deb 

    - name: Copy filebeat configuration
      copy:
       src: /etc/ansible/files/filebeat-configuration.yml
       dest: /etc/filebeat/filebeat.yml
       owner: root
       group: root
       mode: '0600'
       backup: yes

    - name: restart filebeat
      shell: filebeat modules enable system
   
    - name: filebeat setup
      shell: filebeat setup
  
    - name: filebeat -e
      shell: filebeat -e &
      ```
