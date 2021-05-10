		DEPLOY WORDPRESS

I. PREP PHASE:

- [ ] Set Up Blank Ubuntu 20.04 Nodes:
	**Practice 1:**
	1. Deployment Node
	2. Master Node

	**Practice 2:**
	1. WordPress Node
	2. MariaDB Node
	3. Master Node


*Note*:
> **Master Node** is where Ansible is executed.

- [ ] List all required commands


II. WORK ON ANSIBLE MODULES:

- ssh-keygen & copy-key:
````
$ ssh-keygen

$ ssh-copyid <user-name>:<ip-address>
````

A. PRACTICE 1: `All-in-one Deployment`

1. Set up environment:
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



III. DOCUMENTATION:
- [ ] Write .md Documentation
- [ ] Draw diagrams for each Architecture:
	+ Multinode 
	+ Single node
