# WEEK-4 PRACTICE DOCUMENTATION

## Microservices & CI/CD
---

## **Author:** *Julian (Phong) Ng.* 

**Date of issue**: *June 4th 2021*

> Yes, the final week is here! 5th training project in a row at **Viettel Network**! Enjoy ur time :smile_cat:. Feel free to hit me up if any edition is needed!

---

# TABLE OF CONTENTS

## [I. Overview](#I.-OVERVIEW)
  
  - ### [1. CI](#1.-CI)

  - ### [2. CD](#2.-CD)
  
  - ### [3. Web Server](#3.-Web-Server)


## [II. Requirements](#II.-PREREQUISITE:)
  
  - ### [Knowledge](#Knowledge)
  - ### [Technical](#Technical)

## [III. Architecture](#)
  
  - ### [Web Server](#Web-Server)
  - ### [CI/CD Work Flow](#CI/CD-WORK-FLOW)
  - ### [Infrastructure](#INFRASTRUCTURE)

## [IV. Step-by-step Guide](#IV.-STEP-BY-STEP-GUILD)
  
  - ### [A. Web Server](#A.-Web-Server)
  
  - ### [B. Continuous Integration](#B.-Continuous-Integration)

  - ### [C. Continuous Delivery](#C.-Continuous-Delivery)

## [V. Troubleshooting](#V.-TROUBLESHOOTING)

## [VI. References](#VI.-REFERENCES)

# I. OVERVIEW:

## Continuous Integration - Continueous Delivery (CI/CD)
- CI Story: wait for the code base of project is built --> deployed to staging server --> Testing
 => Tons of bugs

### `Jenkins`

- What is a **Jenkin Plugin**

## Web Server
### NodeJS
- CI Story: wait for the code base of project is built --> deployed to staging server --> Testing
 => Tons of bugs

- What is a **Jenkin Plugin**

## II. REQUIREMENTS

### A. Knowledge

### B. Technical

#### Infrastructure

#### Tech stack: 

**Software Development**
- Programming Language: Javascript
- Framework: 
   - Express
- Runtime Environment: NodeJS 14.7.0
- NoSQL Database: MongoDB Atlas
- API Testing: Postman 

**DevOps**
- CI/CD: 
  - Jenkins
  - Ansible
- Hypervisor: VMware Workstation
- Containerization: Docker
  - Image Registry: Docker Hub
    - [**NodeJS App**](https://hub.docker.com/repository/docker/pnguyen01/simple-to-do-nodejs-app)
    - [**NodeJS Environment**](https://hub.docker.com/repository/docker/pnguyen01/node-docker)
....

## III. ARCHITECTURE

### [Web Server](#Web-Server)
### [CI/CD Work Flow](#CI/CD-WORK-FLOW)
### [Infrastructure](#INFRASTRUCTURE)

# Environment & Infrastructure Set up:


## Hardware Requirements:


## Software for installation:


# **STEP-BY-STEP GUIDE**:

**Note**: **Planning saves time!** It is easy to get lost while working on a greater size project, and setting specific milestones overhead saves your life.

#### **:construction: Deployment Idea**
This project can be divided into 2 main sections: 
  - Web Server Development: *yes, we gonna do lots of coding here*
  - CI/CD: *but this one is the Protagonist of the whole project* 

Within `CI/CD`, there are 5 main stages:
  - Continuous Integration:
    1. Build Project:
    2. Unit Test
    3. Build Image 
    4. Publish Image

  - Continuous Delivery:
    4. Deploy

#### **:construction: Directory Structuring**
```bash

```

## **A. Web Server**

## `Express` Web Server: *Super-simple-to-do-app*

## Container
- Create `Dockerfile`

### CI
- Create `Jenkinsfile`
  - Explain terms: [Explain terms](https://www.jenkins.io/doc/book/glossary/#:~:text=Agent,Build)

#### **Directory Structuring**
```yaml

```

## **B. Continuous Integration - Continuous Delivery**

### **1. Continuos Integration**

#### **Deployment Ideas** *Before moving ahead, this section can be seperated in to smaller chunks.*
  
  **1. Enviroment Setup & `Jenkins` Installation**: prepare the workspace for `Continuous Integration` pipeline.
  
  **2. Configure `Jenkins` after Installation & Set up `Pipeline`**
  
  **3. `Continuous Integration`**: including main tasks of this part. They should be executed sequentially as below.

```
  Build Project ----->  
                  Unit Test -----> 
                              Build Image -----> 
                                             Publish Image
```

#### **Enviroment Setup & `Jenkins` Installation**: 

**Note**: To install `Jenkins`, there are 2 recommended options: via `apt`, via `Docker`.
This deployment uses **Docker Container** to run `Jenkins`.

**Docker Container Deployment Model**: 2 containers
<dl>
    <dt>
      <code>docker-jk-third</code>
    </dt>
    <dd>
      Docker-in-docker Container
    </dd>
    <dt>
      <code>jenkins-third</code>
    </dt>
    <dd>
        Running Jenkins on Container. Image of this container is customized.
    </dd>
</dl>  

- Enviroment Setup before installing `Jenkins`:
	- Update `apt`:
  
  ```bash
  $ sudo apt-get update
  ```	

	- Install `Docker` on host machine:

	```bash
  $ sudo apt install docker.io
  ```

- `Jenkins` Installation:

  - Pulling & Running `docker:dind` (**Docker-in-Docker**) image: *yes, you are not getting it wrong. A Docker environment is about to be ran within another Docker Container :zany_face:*

  ```bash
  $ sudo docker run \
    --name docker-jk-third \
    -d \
    --privileged \
    --network host \
    --env DOCKER_TLS_CERTDIR=/certs \
    --volume docker-certs-jk:/certs/client \
    --volume persistence-jk:/var/jenkins_home \
    docker:dind \
    --storage-driver overlay2 
  ```

  **Explain fields**
  <dl>
    <dt>
      <b>--privileged</b>
    </dt>
    <dd>
        Running container in <code>privileged</code> mode - allows container to acquire resources/capabilities of host machine. <b>Be catious before using!</b>
    </dd>
    <dt>
      <b>--network</b> host
    </dt>
    <dd>
        Container's network share same namespace with host machine
    </dd>
    <dt>
      <b>--volume</b> persistence-jk:/var/jenkins_home
    </dt>
    <dd>
        Mounting volume <code>persistence-jk</code> to workspace directory of cJenkins
    </dd>
</dl>  


  - Build Customized `Jenkins` Image: *A bit of handy work here.*
  	- Create `Dockerfile`:
      - Image: [**jenkins/jenkins**](https://hub.docker.com/r/jenkins/jenkins/).\
      (*Do not use [**jenkins**](https://hub.docker.com/_/jenkins) - Already **deprecated***)
  	
  	```bash
  	$ vi Dockerfile

  	----
    
  	FROM jenkins/jenkins:2.277.4-lts-jdk11
  	USER root
  	RUN apt-get update && apt-get install -y apt-transport-https \
         	ca-certificates curl gnupg2 \
         	software-properties-common
  	RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
  	RUN apt-key fingerprint 0EBFCD88
  	RUN add-apt-repository \
         "deb [arch=amd64] https://download.docker.com/linux/debian \
         $(lsb_release -cs) stable"
  	RUN apt-get update && apt-get install -y docker-ce-cli               #Install Docker
  	USER jenkins              #Run this image as 'jenkins' user
  	```
      **Note**: *Tried to install `BlueOcean` plugins (GUI for CI/CD Pipeline) but did not succeed.*
sssss

  	- Navigate to directory with `Dockerfile` & Build **'jenkins-cus'** image:

    ```sh
    $ docker build -t jenkins-cus:<some-tag> .
    ```

    - (Pulling &) Running `Jenkins` container:

    ```bash
    $ sudo docker run \
      --name jenkins-third \
      -d \
      --network host \
      --env DOCKER_HOST=tcp://localhost:2376 \
      --env DOCKER_CERT_PATH=/certs/client \
      --env DOCKER_TLS_VERIFY=1 \
      -v persistence-jk:/var/jenkins_home \
      -v docker-certs-jk:/certs/client:ro \
      jenkins-cus:0.1
    ```
  
    **Explain fields**
      <dl>
        <dt>
          <b>--network</b> host
        </dt>
        <dd>
            Container's network share same namespace with host machine
        </dd>
        <dt>
          <b>--volume</b> persistence-jk:/var/jenkins_home
        </dt>
        <dd>
          Mounting volume <code>persistence-jk</code> to workspace directory of Jenkins
        </dd>
      </dl>  

:heavy_check_mark: **Expected outcome**: *2 containers running*

<img src="./imgs/container-started.png">

#### **Configure `Jenkins` after Installation & Set up `Pipeline`**:

- Access dashboard at: [**`http://192.168.80.133:8080/`**](http://192.168.80.133:8080/)

<img src="./imgs/jenkins-init-dashboard.png">

- View logs of `jenkins-third` container for password & sign in `Jenkins` Dashboard:

```bash
$ docker logs jenkins-third
```

<img src="./imgs/jenkins-init-pass.png">

- Choose `Install Suggested Plugins` & Download Plugins:

<img src="./imgs/dashboard-install-suggested-plugins.png">
  
  - Plugin Download in progress:

	<img src="./imgs/dashboard-install-suggested-plugins-downloading.png">

- Create `Admin User` on Dashboard:
<img src="./imgs/create-admin-user.png">

- `Jenkins` ready to roll :metal:
<img src="./imgs/jenkins-ready.png">

- Install essential `Plugins`: Following **Plugins** must be satisfied
  - **Docker**
  - **Pipeline** 

  **Note**
*There are multiple plugins to install as `Jenkins` is designed to be highly modular. Errors are expected to occur during these installations.*

  :sunglasses: **Keep it cool if seeing this. Everything is under control**
  <img src="./imgs/missing-plugins.png">

- Create a `Pipeline` on `Jenkins` :
  <img src="./imgs/main-dashboard-jenkins.png">

	- Create job:
	<img src="./imgs/create-job.png">

  - Assign `Name` & Choose `Pipeline`:
  <img src="./imgs/create-job-name-type.png">

  - Configure `metadata` & Add GitHub Repository to `Pipeline`:
   <img src="./imgs/connect-jenkin-github.png">


#### **`Continuous Integration`**: :sunglasses: *Well, the fun begins...*

- **Requirements**: *To proceed, please ensure the following items are satisfied*

  - **Plugins**: these **plugins** below must be installed
    - Docker API Plugin
    - Docker Commons Plugin
    - Docker Pipeline
    - Docker Plugin

  - **Source Code for NodeJS Application**: RESTful API with NodeJS be ready. The source code should be published to `GitHub`.

  - **DockerHub Account**: Register for an account on [**DockerHub**](https://hub.docker.com/)

- Adding `DockerHub Credentials` to `Jenkins`'s Dashboard:
  - Navigate to `Global Credentials`:
    <img src="./imgs/nav-credentials.png">
    
  - Enter `username` & `passwords` of personal `DockerHub` account:
    - TODO: `Credential ID` can be any string, it is named `docker_hub_cred` in this deployment.
    <img src="./imgs/credentials-added.png">

  - Credential added successfully:
  <img src="./imgs/credentials-added.png">

- Configure `Jenkinsfile`:
  - Navigate to the Repository of application & create (if not done before)/configure `Jenkinsfile`: *Below is a not-that-optimized pipeline that written in `declarative` format. With advanced implementations, `scripted pipeline` is highly recommended.*

  ```groovy
  $ vi <path-to-app-dir>/jenkins/Jenkinsfile

  ----
    pipeline {

        agent none 

        environment {
          registry = "pnguyen01/simple-to-do-nodejs-app"        //DockerHub - Registry for storing Docker Image
          registryCredential = 'docker_hub_cred'                //Maps to previously defined
          dockerImage = ''
          CI = 'true'
          HOME = '.'
        }

        stages {

            stage('Build Project') {
          agent {
              docker {
                reuseNode true
                image 'pnguyen01/node-docker:1.1'            // Docker image for testing environment
                args '-p 3400:3400 --privileged -v /var/run/docker.sock:/var/run/docker.sock'
              }
          }
                steps {
                    sh './jenkins/scripts/build.sh' 
                }
            }

            stage('Basic Test') {
          agent {
              docker {
                  reuseNode true
                  image 'pnguyen01/node-docker:1.1'            // Docker image for testing environment
                  args '-p 3400:3400 --privileged -v /var/run/docker.sock:/var/run/docker.sock'
              }
                }
                steps {
                    sh './jenkins/scripts/test.sh'
                }
            }

      stage('Building Image') {
        agent {
                      docker {
                              reuseNode true
                              image 'pnguyen01/node-docker:1.1'            // Docker image for testing environment
                              args '-p 3400:3400 --privileged -v /var/run/docker.sock:/var/run/docker.sock'
                      }
                }
        steps {
          script {
            dockerImage = docker.build registry + ":latest" 
          }
        }
      }

      stage('Push Image to DockerHub') {
        agent {
              docker {
                reuseNode true
                image 'pnguyen01/node-docker:1.1'            // Docker image for testing environment
                args '-p 3400:3400 --privileged -v /var/run/docker.sock:/var/run/docker.sock'
              }
        }
        steps {
          script {
            docker.withRegistry('', registryCredential) {
              dockerImage.push()
            }
          }
        }
      }

      stage('Remove built image from local machine') {
        agent {
                            docker {
                                    reuseNode true
                                    image 'pnguyen01/node-docker:1.1'            // Docker image for testing environment
                                    args '-p 3400:3400 --privileged -v /var/run/docker.sock:/var/run/docker.sock'
                            }
                    }

        steps {
          sh './jenkins/scripts/remove-img.sh'
        }
      }
    }

  ```

- Enter `docker-jk` container:
**Note**: *May need to repeat every stop/start container*

:heavy_check_mark: **Expected Console Output**

- `Build`: Source code is cloned from personal public GitHub Repo. Project is built using `npm` - [*Node Package Manager*](https://www.npmjs.com/) to install dependencies.

```bash
....
+ ./jenkins/scripts/build.sh
-----------------------------------------------
						     
						     
I. Phase One
						     
==============Building Project=================
						     
-----------------------------------------------
						     
Installing Packages via `npm`...
+ npm install
npm WARN to-doapp@1.0.0 No repository field.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@2.3.2 (node_modules/fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@2.3.2: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"x64"})

audited 236 packages in 3.772s

19 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities

[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Test)
[Pipeline] sh
```

- `Unit Test`:

```bash
+ ./jenkins/scripts/test.sh
-----------------------------------------------
						     
                 						     
II. Phase Two

=============Running Test cases================
-----------------------------------------------
+ npm test

> to-doapp@1.0.0 test /var/jenkins_home/workspace/to-do-app-nodejs
> mocha --timeout 10000 --exit -R spec

(node:60) Warning: Accessing non-existent property 'search' of module exports inside circular dependency
(Use `node --trace-warnings ...` to show where the warning was created)
Application is running on port 3400


  Board CRUD
    /api/auth/login - Log in User
      ✓ it should authenticate an exisiting user & retrieve JWT Token (1881ms)
    /api/boards/create - Creating new Board
      ✓ it should create a new board (153ms)
    /api/boards/update/:id - Update Board
      ✓ it should update 'title' & 'description' of board (141ms)
    /api/boards/:id - Board
      ✓ it should return the board with defined ID (146ms)
    /api/boards/list - Board
      ✓ it should return board list of an User (131ms)
    /api/boards/delete - Delete Board
      ✓ it should delete an existing board (161ms)

  Task CRUD
    /api/auth/login - Log in User
      ✓ it should authenticate an exisiting user & retrieve JWT Token (550ms)
    /api/boards/create - Creating new Board
      ✓ it should create a new board (150ms)
    /api/tasks/create - Creating new Task
      ✓ it should create a new task (213ms)
    /api/tasks/update/:id - Update Task
      ✓ it should update 'title' & 'description' of task (125ms)
    /api/tasks/:id - Get Task
      ✓ it should return the Task with defined ID (125ms)
    /api/tasks/list - Task List
      ✓ it should return task list of an User (117ms)
    /api/tasks/delete - Delete Task
      ✓ it should delete an existing Task (131ms)

  User Authentication
    /api/auth/signup - Sign up User
      ✓ it should add a new user (509ms)
    /api/auth/login - Log in User
      ✓ it should authenticate an exisiting user (488ms)


  15 passing (5s)

[Pipeline] }
....
```

- `Build Image`: execute commands to `containerize` the NodeJS Application:

```bash
------
+ docker build -t pnguyen01/simple-to-do-nodejs-app:latest .
Sending build context to Docker daemon  519.2kB

Step 1/7 : FROM node:lts
 ---> 9153ee3e2ced
Step 2/7 : WORKDIR /usr/src/app
 ---> Using cache
 ---> a15134f4644d
Step 3/7 : COPY package*.json ./
 ---> Using cache
 ---> 49f32decbb18
Step 4/7 : RUN npm install
 ---> Using cache
 ---> 71a6f34822b5
Step 5/7 : COPY . .
 ---> 83e51a8d6dcd
Step 6/7 : EXPOSE 3400
 ---> Running in af217f701072
Removing intermediate container af217f701072
 ---> 4c6b3632ce39
Step 7/7 : CMD [ "node", "server.js" ]
 ---> Running in 878a8ee9dd2a
Removing intermediate container 878a8ee9dd2a
 ---> f93dd8cad25b
Successfully built f93dd8cad25b
Successfully tagged pnguyen01/simple-to-do-nodejs-app:latest
[Pipeline] }
```

- `Publish Image`: Push the `containerized` [To-do NodeJS Application](https://hub.docker.com/repository/docker/pnguyen01/simple-to-do-nodejs-app) to **DockerHub**:

```bash
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Push Image to DockerHub)
[Pipeline] script
[Pipeline] {
[Pipeline] withEnv
[Pipeline] {
[Pipeline] withDockerRegistry
$ docker exec --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** --env ******** ******************** docker login -u pnguyen01 -p ******** https://index.docker.io/v1/

Login Succeeded
[Pipeline] {
[Pipeline] isUnix
[Pipeline] sh
+ docker tag pnguyen01/simple-to-do-nodejs-app:latest pnguyen01/simple-to-do-nodejs-app:latest
[Pipeline] isUnix
[Pipeline] sh
+ docker push pnguyen01/simple-to-do-nodejs-app:latest
The push refers to repository [docker.io/pnguyen01/simple-to-do-nodejs-app]
020b802e3ae5: Preparing
0c46acf3eacd: Preparing
59a0a9c1f408: Preparing
d91648fbd134: Preparing
b238f928d38b: Preparing
4a844761bb65: Preparing
b1501adb3037: Preparing
b257e69d416f: Preparing
1e9c28d06610: Preparing
cddb98d77163: Preparing
ed0a3d9cbcc7: Preparing
8c8e652ecd8f: Preparing
2f4ee6a2e1b5: Preparing
ed0a3d9cbcc7: Waiting
8c8e652ecd8f: Waiting
2f4ee6a2e1b5: Waiting
4a844761bb65: Waiting
b1501adb3037: Waiting
b257e69d416f: Waiting
1e9c28d06610: Waiting
cddb98d77163: Waiting
b238f928d38b: Layer already exists
4a844761bb65: Layer already exists
b1501adb3037: Layer already exists
b257e69d416f: Layer already exists
d91648fbd134: Pushed
59a0a9c1f408: Pushed
1e9c28d06610: Layer already exists
020b802e3ae5: Pushed
cddb98d77163: Layer already exists
ed0a3d9cbcc7: Layer already exists
8c8e652ecd8f: Layer already exists
2f4ee6a2e1b5: Layer already exists
0c46acf3eacd: Pushed
latest: digest: sha256:1c97fc6023d5aea35e48d16ff53e26ba26c3f673111ab04a3aa0065bb3fdb051 size: 3053
[Pipeline] }
[Pipeline] // withDockerRegistry
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
$ docker stop --time=1 2bdcd92e8c4d541ea918e80c41b19544c66f28ba88d170d0bc19390496a9de3f
$ docker rm -f 2bdcd92e8c4d541ea918e80c41b19544c66f28ba88d170d0bc19390496a9de3f
[Pipeline] // withDockerContainer
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```

**:partying_face: Congratulations, it runs. CI down. 1 more to go**

```bash

$ chmod 666 /var/run/docker.sock
```

- Expected Outcomes: [Sample NodeJS DockerHub](https://hub.docker.com/repository/docker/pnguyen01/simple-to-do-nodejs-app)

## CD:
### Environment Setup
- Configure host-machine (jenkins-slave):
  - Install packages for `jenkins`
  ```bash
  $ sudo apt-get install -y openjdk-8-jdk sshpass
  ```
  - Generate SSH key:
  ```bash
  $ ssh-keygen
  ```

  - Add Public key:
  ```bash
  $ sudo mkdir -p /var/lib/pnguyen/.ssh 
  $ sudo cp /home/pnguyen/.ssh/id_rsa.pub /var/lib/pnguyen/.ssh/authorized_keys
  ```
  
  - Retrieve `private key`:
  ```bash
  $ cat /home/pnguyen/.ssh/id_rsa
  ```

  - Add `Private key` to `Credentials` on `Jenkins` Dashboard:

  - Add `Agent` on `Jenkins` Dashboard


## Troubleshooting

### Jenkins:
1 `Invalid agent type "docker" specified....`
- Install following Plugins:
	- Docker
	- Docker Pipeline

2. `Permission Denied`

```bash
+ ./jenkins/scripts/build.sh
/var/jenkins_home/workspace/to-do-app-nodejs@tmp/durable-c46bcad3/script.sh: 1: /var/jenkins_home/workspace/to-do-app-nodejs@tmp/durable-c46bcad3/script.sh: ./jenkins/scripts/build.sh: Permission denied
```

- Add permission:

```bash
$ git update-index --chmod=+x <Target-file (path can be included)>
```

3. `Cannot connect to the Docker daemon at unix:/var/run/docker.sock. Is the docker daemon running?`
- Change permission:
```bash
$ chmod 666 /var/run/docker.sock
```
 

4. Unable to use `npm`:
```bash

```


## References

[Installing Jenkins with Docker](https://www.jenkins.io/doc/book/installing/docker/)

[Build a NodeJS app & React app with `npm`](https://www.jenkins.io/doc/tutorials/build-a-node-js-and-react-app-with-npm/)

[Build a Java app with Maven](https://www.jenkins.io/doc/tutorials/build-a-java-app-with-maven/)

[Building NodeJs Application with Docker and Deploying to Jenkins](https://www.youtube.com/watch?v=45YX7hyZSo4)

[BlueOcean Plugins](https://plugins.jenkins.io/blueocean/)

[The official Docker Image for Jenken from `Docker Hub`](https://www.jenkins.io/blog/2018/12/10/the-official-Docker-image/)

[Integrating Ansible - Jenkins CI/CD process](https://www.redhat.com/en/blog/integrating-ansible-jenkins-cicd-process)

[Building Docker Images to Docker Hub using Jenkins Pipelines](https://dzone.com/articles/building-docker-images-to-docker-hub-using-jenkins#:~:text=Setting%20Up%20Your%20Environment,definition%20to%20your%20Github%20repository.)

[Building Images for Ansible](https://geektechstuff.com/2020/02/10/ansible-in-a-docker-container/)

[Bulding Jenkins Inside Ephemeral Docker Container](https://technology.riotgames.com/news/building-jenkins-inside-ephemeral-docker-container)

[What is BlueOcean](https://www.jenkins.io/projects/blueocean/about/)

### Troubleshooting:
- [Bash file - Permission Denied](https://stackoverflow.com/questions/46766121/permission-denied-error-jenkins-shell-script)
