# DOCUMENTATION ON TAKE-HOME PRACTICES
---
## **Author:** *Julian (Phong) Ng.*
**Date of issued**: *May 4th 2021*

## Requirements:
**1. System**:
- Ubuntu Server Image (**Ubuntu 20.04** is used in below practices: [Download image](https://ubuntu.com/download/server))
- Desktop hypervisor for running virtual machines (E.g: *Virtual Box, VM Workstation, etc*)

**2. Knowledge**:
- Basic Linux, Networking concepts.
- Basic Docker usage.

## System Configuration on VM(s):

### 1. Install essential packages via `apt`:
- Update apt (*Well, better do this all the time*):
```
 $ sudo apt-get update -y
```
- Install some neccessary packages:
```
$ sudo apt-get install -y vim net-tools systemd git curl  
```

### 2. Install Docker CE:

Please refer to this official doc for further details [Install Docker CE Ubuntu](https://docs.docker.com/engine/install/ubuntu/).

### **Note:** 
*Docker CE is installed via Repository in below practices*

### Basic must-know diagnostic commands:
### A. Docker:
**1. View Logs of container**:

```
$ docker logs <Container's id/name>
```
<dl>
  <dt>
  </dt>
</dl>

### B. Linux:

## I. Practice 1: `Deploy WordPress with Command Line`

### A. Step-by-step:

**1. Create network:**
```
$ docker network create wordpress-network
```

**2. Create a volume for MariaDB persistence:**
```
$ docker volume create --name mariadb_data
```

**3. Run MariaDB container:**
```
$ docker run -d --name mariadb \
  --env ALLOW_EMPTY_PASSWORD=yes \
  --env MARIADB_USER=bn_wordpress \
  --env MARIADB_DATABASE=bitnami_wordpress \
  --network wordpress-network \
  --volume mariadb_data:/bitnami/mariadb \
  bitnami/mariadb:latest
```


**4. Create a volume for WordPress persistence:**
```
$ docker volume create --name wordpress_data
```

**5. Run WordPress container:**
```
$ docker run -d --name wordpress \
  -p 8080:8080 -p 8443:8443 \
  --env ALLOW_EMPTY_PASSWORD=yes \
  --env WORDPRESS_DATABASE_USER=bn_wordpress \
  --env WORDPRESS_DATABASE_NAME=bitnami_wordpress \
  --network wordpress-network \
  --volume wordpress_data:/bitnami/wordpress \
  bitnami/wordpress:latest
```

### B. Expected Outcomes:
> Running Containers:

<img src="./imgs/hw1 - docker.png">

> Landing Page:
<img src="./imgs/hw1 - page.png">

## II. Practice 2: `Deploy WordPress with Docker Compose`

### A. Step-by-step:

**1. Install `docker-compose`:** *can install via `apt`* 
```
$ sudo apt install docker-compose
```

**2. Download `docker-compose.yml` of WordPress project**
```
$ curl -sSL https://raw.githubusercontent.com/bitnami/bitnami-docker-wordpress/master/docker-compose.yml > docker-compose.yml
```

**3. Run application:**
```
$ docker-compose up -d
```

### B. Expected Outcomes:
> Running Containers:

<img src="./imgs/hw 2 - docker.png">

> Landing Page:

<img src="./imgs/hw 2 - page.png">

## III. Practice 3: `Deploy WordPress with Command Line on two virtual machines`
### **Note:** 
  - Requires 2 Ubuntu Nodes. Thus, basic configuration must be done on both machines.*
  - *In order to save time, **Cloning VM** is an option*

### A. Step-by-step guide:

> Presume that environment on both nodes are ready. 

> Some steps can be skipped if re-use VM from previous Practices.

#### **a. Database Node `(MariaDB)`**:
**1. Set Hostname:**
```
$ hostnamectl set-hostname mariadb
```

**2. Create network:**
```
$ docker network create wordpress-network
```

**3. Create a volume for MariaDB persistence:**
```
$ docker volume create --name mariadb_data
```

**4. Run MariaDB container:**
```
$ docker run -d --name mariadb \
  --env ALLOW_EMPTY_PASSWORD=yes \
  --env MARIADB_USER=bn_wordpress \
  --env MARIADB_DATABASE=bitnami_wordpress \
  --env MARIADB_PASSWORD=12345678 \
  --network wordpress-network \
  --volume mariadb_data:/bitnami/mariadb \
  bitnami/mariadb:latest
```

#### **b. WordPress Node**:

**1. Create a volume for WordPress persistence:**
```
$ docker volume create --name wordpress_data
```

**2. Run MariaDB container:**
```
$ docker run -d --name wordpress  -p 8080:80 -p 8443:8443   --env MARIADB_HOST=192.168.80.132   --env MARIADB_PORT_NUMBER=3306   --env WORDPRESS_DATABASE_NAME=bitnami_wordpress   --env WORDPRESS_DATABASE_USER=bn_wordpress   --env WORDPRESS_DATABASE_PASSWORD=12345678 --entrypoint="/app-entrypoint.sh" --add-host mariadb:192.168.80.132   --volume wordpress_data:/bitnami/wordpress  bitnami/wordpress:mod httpd -f /opt/bitnami/apache/conf/httpd.conf -DFOREGROUND
```

**Explain**:
<dl>
  <dt>--add-host</dt>
  <dd>
    Map hostname to IP address of MariaDB node in 
    <code>/etc/hosts</code>
  </dd>
  <dt>--entrypoint</dt>
  <dd>
    Set the path to entrypoint file for container.
  </dd>
  <dt>
    httpd -f /opt/bitnami/apache/conf/httpd.conf -DFOREGROUND
  </dt>
  <dd>
    Last argument injected as CMD for container. Starting Apache server in this case
  </dd>
</dl>


### B. Debugging:
#### **1. Docker**:

#### **2. System**:

### C. Expected Outcomes:

