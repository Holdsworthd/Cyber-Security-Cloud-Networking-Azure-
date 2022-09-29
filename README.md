# Cyber Security Cloud Networking using Azure

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://drive.google.com/file/d/1FwtlWE8wAn_l9Lcvmn_0XWeAp0VFgnnj/view?usp=sharing

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the ansible-config.yml file may be used to install only certain pieces of it, such as Filebeat.

```
  ---
- name: Configure Elk VM with Docker
  hosts: elk
  remote_user: azadmin
  become: true
  tasks:
    # Use apt module
    - name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present

      # Use apt module
    - name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

      # Use pip module
    - name: Install Docker python module
      pip:
        name: docker
        state: present

      # Use sysctl module
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

      # Use docker_container module
    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044

      # Use systemd module
    - name: Enable service docker on boot
      systemd:
        name: docker
        enabled: yes
```

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting traffic to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the operating system and system logs.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |
| Web 1    |Container | 10.0.0.5   | Linux            |
| Web 2    |Container | 10.0.0.6   | Linux            |
| Web 3    |Container | 10.0.0.7   | Linux            |
|ELK Server| Monitor  | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 52.287.201.221

Machines within the network can only be accessed by Jump Box with IP address 52.287.201.221.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 |    10.0.0.1/16       |
|ELK Server| Yes                 |114.77.33.77 10.0.0.4 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it saves time and ensures that scalability and homogeneity are possible.

The playbook implements the following tasks:
- Installs docker.io
- Installs pip3
- Installs Docker python module
- Allows for the use of more memory
- Downloads and launches a docker elk container
- Enables service docker on boot

The following screenshot displays the result of running `sudo docker ps` after successfully configuring the ELK instance.

https://drive.google.com/file/d/1bQKJhPNlHSQaPTt4DCxmHS0UeNfMWsRv/view?usp=sharing

### Target Machines & Beats
This ELK server is configured to monitor the following machines: 10.0.0.5  10.0.0.6  10.0.0.7

We have installed the following Beats on these machines:Elasticsearch, Kibana

These Beats allow us to collect the following information from each machine:
Elasticsearch: Takes unstructured data from, different location, and then stores and indexes it according rules set by the user, which is then searchable. This allows us to search and analyse this data in close to real time.
Kibana: Allows us to analyse vast amounts of logs and applications, and then visualise that data through the use of easily understandable graphs and images.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat configuration file to etc/ansible/filebeat-config.yml.
- Update the filebeat configuration file to include your ELK server's IP address
- Run the playbook, and navigate to the Filebeat installation page on the ELK server GUI to check that the installation worked as expected.

FAQ
- Which file is the playbook? Where do you copy it? 
	filebeat-playbook.yml to be copied to /etc/ansible/filebeat-config.yml

- Which file do you update to make Ansible run the playbook on a specific machine?
	/etc/filebeat/filebeat-config.yml
- How do I specify which machine to install the ELK server on versus which to install Filebeat on?
	By editing the filebeat-config.yml file
- Which URL do you navigate to in order to check that the ELK server is running? 
	http://20.188.112.188:5601/app/kibana or http://[your.VM.IP]:5601/app/kibana


