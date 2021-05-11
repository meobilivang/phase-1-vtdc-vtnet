# WEEK-2 PRACTICES DOCUMENTATION
# AUTOMATION w/ ANSIBLE
---
## **Author:** *Julian (Phong) Ng.*
**Date of issue**: *May 11th 2021*

> Welcome back! This is the documentation for my second training project at **Viettelnet**. Enjoy ur time :smile_cat:. Feel free to hit me up if any edition is needed!

## **I. GENERAL**:
## 1. KNOWLEDGE:
- Basic on Linux, networking
- Basic on `Ansible`
- Again, hope you familiar with this one :wink:
<img src="./img/vim.jpg">

## 2. Infrastructure Setup:
### **Note:**
> *- Controller Node: where Ansible scripts runs & establish SSH connections to other node(s)*
> *- Managed Node(s): Running WordPress & its data persistence server (`MariaDB`)*
### A. Practice 1: `All-in-one WordPress Deployment` 

### B. Practice 2: `Multinode WordPress Deployment`

I. PREP PHASE:

- Set Up Blank Ubuntu 20.04 Nodes:
	**Practice 1:**
	1. Controlled Node
	2. Managed Node

	**Practice 2:**
	1. WordPress Node
	2. MariaDB Node
	3. Master Node


*Note*:
> **Master Node** is where Ansible is executed.

- [ ] List all required commands


II. WORK ON ANSIBLE MODULES:
- Configure `ansible.conf`:
	+ Make ansible runs faster

- ssh-keygen & copy-key:
````
$ ssh-keygen

$ ssh-copyid <user-name>:<ip-address>
````

- Allow port 22:
$ sudo ufw allow ssh

- Edit `sudoer` on Controller Node:
=> allow user execute all commands
	Note: NOT RECOMMENDED on Production
<user> 

- SSH password-based login with sshpass:
$ sudo apt update

- Configure `hosts` / `hosts.yml`:
	> Specifying Hostname - IP Address


* CONFIGURE CONTROLLER NODE:
- Virtual env
- Paramiko

A. PRACTICE 1: `All-in-one Deployment`
Note:
> Change IP address in `deploy-node`

1. Set up environment:
- [ ] Check connection to Node  

- [ ] Update apt
	$ apt update -y

- [ ] Install dependencies: net-tools, vim, git,...
	$ apt get net-tools vim git 

- [ ] Install Docker (via `apt`):
	Link:
	````
	https://techviewleo.com/ansible-check-if-software-package-is-installed/
	````
		
	````
	https://www.axelerant.com/resources/team-blog/managing-docker-containers-using-ansible
	`````

	````
	https://techviewleo.com/manage-docker-containers-with-ansible/
	````


	+ Dependencies for Docker 

	$ sudo apt-get install curl apt-transport-https ca-certificates software-properties-common

	+ Docker io, ce. 
	$ sudo apt-get install docker-ce docker-ce-cli containerd.io

- [ ] Allows connection on specific ports: 3306, 8080, 8443
+ Open ports:
	

- [ ] Test connection to other Nodes (via `ping`)

2. Docker Commands:

a. MariaDB 
- Create network

- Create volume

- Run container

b. WordPress:
- Create network

- Create volume

- Run container

B, PRACTICE 2: `Multinode`


III. DEBUGGING:
1. `Timeout (12s) waiting for privilege escalation prompt`
- Solution:
	+ Install `paramiko`
	$ pip install paramiko
	
	+ Deploy `playbook`:
	$ ansible-playbook -i hosts -c paramiko site.yml

IV. DOCUMENTATION:
- [ ] Write .md Documentation
- [ ] Draw diagrams for each Architecture:
	+ Multinode 
	+ Single node
- [ ] Structure:
	C:.
├───all-in-one
│   └───roles
│       ├───common
│       │   └───tasks
│       └───docker
│           └───tasks
└───multinode
    └───roles
        ├───common
        │   └───tasks
        ├───mariadb
        │   └───tasks
        └───wordpress
            └───tasks
