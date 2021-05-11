# WEEK-2 PRACTICES DOCUMENTATION
# AUTOMATION with ANSIBLE
---
## **Author:** *Julian (Phong) Ng.*
**Date of issue**: *May 11th 2021*

> Welcome back! This is the documentation for my second training project at **Viettelnet**. Enjoy ur time :smile_cat:. Feel free to hit me up if any edition is needed!

# **I. OVERVIEW**:
## 1. KNOWLEDGE:
- Basic on System Administration (Linux, Networking, etc)
- Basic on `Ansible`:
> *Ansible is an open source community project sponsored by Red Hat, it's the simplest way to automate IT. Ansible is the only automation language that can be used across entire IT teams from systems and network administrators to developers and managers.*
- Again, hope you familiar with this one :wink: :

<img src="./img/vim.jpg">

## 2. Infrastructure Setup:

### **Terms Explained:**
> **Controller Node**: where Ansible scripts runs & establish SSH connections to other node(s) \
> **Managed Node(s)**: Running WordPress & its Data Persistence server (`MariaDB`)

### **Requirements**:
- Ubuntu Server Image (**Ubuntu 20.04** is used in below practices: [Download image](https://ubuntu.com/download/server))

- Desktop Hypervisor for running virtual machines (E.g: *Virtual Box, VM Workstation, etc*)

#### **Note:**
> *VMWare Workstation is **highly recommended** to use.*

### A. Practice 1: `All-in-one WordPress Deployment` 

<img src="./img/aio-diagram.png">

> In my **local deployment**, Nodes have following addresses.

### 1. Controller Node:
-  IP: `192.168.80.135`

### 2. Managed Node: 
> Running both `WordPress` & `MariaDB` containers
-  IP: `192.168.80.136`

### B. Practice 2: `Multinode WordPress Deployment`

<img src="./img/multinode-diagram.png">

### 1. Controller Node:
-  IP: `192.168.80.135`

### 2. Managed Node:

#### a. MariaDB:
-  IP: `192.168.80.136`

#### b. WordPress:
-  IP: `192.168.80.137`

# II. SET UP `Controller Node`:
## 1. Update `apt` & Install essentials packages:
-  Update `apt`:
```
$ sudo apt-get update -y
```
- Install packages:
```
$ sudo apt-get install -y vim net-tools systemd git curl  
```

## 2. (Optional) Install `python3`:
```
$ sudo apt install software-properties-common
$ sudo apt install python3.8
```

## 3. Install `virtualenv`:
**Note**:
> `virtualenv` (*Python's isolated environment*) is used to install `Ansible` in this deployment. 
```
$ sudo apt install python-virtualenv
```

## 4. Start an `virtualenv` & install `Ansible` via `pip`:

```
$ python -m virtualenv ansible-test
$ source ansible-test/bin/activate 						#Activating virtualenv
$ python -m pip install ansible
```

#### Note:
> Only able to use `ansible` when virtualenv is `activated`.

## 5. Install `sshpass`: 
> SSH password-based login. User can specify `ssh` password in inventory file(s).

```
$ sudo apt-get install -y sshpass
```

## 6. Configure `ansible.conf`:
Below configuration can be use for reference
```
[defaults]
host_key_checking = False				
remote_user = pnguyen
timeout = 30

[ssh_connection]
pipelining = True
```

**Fields**:
<dl>
    <dt>
      host_key_checking
    </dt>
    <dd>
       Prompting for confirmation of the key. (<b>Default</b>: true)
    </dd>
</dl>


> Instead of setting `host_key_checking = False`,  `paramiko` package is an alternative.
```
$ pip install paramiko
$ ansible-playbook -i <inv-file> -c paramiko <entry-point> 			#Running playing w/ paramiko
```

# III. ANSIBLE MODULES STEP-BY-STEP:

### Before Proceeding:
> Ensure that all the Nodes has been properly **CREATED** & **BOOTED**.

## A. General Configurations:
> Done on all `Managed Node`

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


- Configure `hosts` / `hosts.yml`:
	> Specifying Hostname - IP Address

A. PRACTICE 1: `All-in-one Deployment`
Note:
> Change IP address in `deploy-node`

1. Set up environment:
-  Check connection to Node  

-  Update apt
	$ apt update -y

-  Install dependencies: net-tools, vim, git,...
	$ apt get net-tools vim git 

-  Install Docker (via `apt`):
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

-  Allows connection on specific ports: 3306, 8080, 8443
+ Open ports:
	

-  Test connection to other Nodes (via `ping`)

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
- Write .md Documentation
- Draw diagrams for each Architecture:
	+ Multinode 
	+ Single node
- Structure:
```
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
```