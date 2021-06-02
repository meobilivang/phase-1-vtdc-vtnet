- Logs of `Jenkins`

## Jenkins:
- Withou CI Story: wait for the code base of project is built --> deployed to staging server --> Testing
 => Tons of bugs

- What is a **Jenkin Plugin**

# Tech stack:
## Software Development
- NodeJS:
	- Express
	
- Database: MongoDB Atlas
- Testing: Postman

## DevOps
- Jenkins
- VMware Workstation
- Docker
- 

## DevOps:
- VMware Workstation 
- Docker
- Jenkins
....

# Environment & Infrastructure Set up:

## Hardware Requirements:


## Software for installation:


# Step-by-step guide:

## **1. Continues Integration - Jenkins**

### Jenkins: 
**Note**
> Using Docker Container
- Available Options:
	- via `apt`
	- `Docker`

- Used Images [**jenkins/jenkins**](https://hub.docker.com/r/jenkins/jenkins/)

- Before install `Jenkins`:
	- Update `apt`:
	```sh

	$ sudo apt-get update
	```
	- Install `Docker`:
	```sh
	$  sudo apt install docker.io
	```
- Create `bridge` network:
```bash
$ sudo docker network create jenkins
```

- Pulling & Running `Docker` container:
```sh
$ sudo docker run \
  --name docker-jk \
  -d \
  --privileged \
  --network jenkins-net \
  --network-alias docker-jk \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume docker-certs-jk:/certs/client \
  --volume persistence-jk:/var/jenkins_home \
  --publish 3000:3000 \
  --publish 2376:2376 \
  docker:dind \
  --storage-driver overlay2 
```
**Explain fields**

- Build Customized `Jenkins`:
	- Create `Dockerfile`:
	
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
	RUN apt-get update && apt-get install -y docker-ce-cli
	USER jenkins
	```

	- Build:
	```sh
	$ docker build -t jenkins-cus:0.1 .
	```

- Pulling & running `Jenkins` container:
```bash

$ sudo docker run \
  --name jenkins \
  -d \
  --network jenkins-net \
  --network-alias jenkins \
  --env DOCKER_HOST=tcp://docker-jk:2376 \
  --env DOCKER_CERT_PATH=/certs/client \
  --env DOCKER_TLS_VERIFY=1 \
  -p 8080:8080 \
  -v persistence-jk:/var/jenkins_home \
  -v docker-certs-jk:/certs/client:ro \
  jenkins-cus:0.1
```

**Explain fields**


*Expected outcome*:
> 2 containers running.

### 2. Configure & set up Jenkins after `Deployment`:

- Access dashboard at: 
	[`http://localhost:8080/`](http://localhost:8080/)	|   [`http://192.168.80.133:8080/`](http://192.168.80.133:8080/)

<img>

- View logs of `jenkins` container for password:
```bash

$ docker logs jenkins
```
<img>

- Use the retrieved password to `unlock` Jenkins:

- Choose `Install Suggested Plugins` & Download Plugins:
<img src="./imgs/dashboard-install-suggested-plugins">

	- Downloading Plugins:
	<img src="./imgs/dashboard-install-suggested-plugins-downloading">

- Create `Admin User` on Dashboard:
```bash
$ 
```
<img src="">


- Integrating `GitHub` with `Jenkins`:
	- Create job:
	
	- Add metadata & Add GitHub Repository:

- (Optional) Install `Blue Ocean` Plugins: *Better UI*


- Console output:
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
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
$ docker stop --time=1 11d31a60f597106dfafd169ea0fef96f0406ed4e3194e388b75d7f68ae26369e
$ docker rm -f 11d31a60f597106dfafd169ea0fef96f0406ed4e3194e388b75d7f68ae26369e
[Pipeline] // withDockerContainer
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```
- Build & Push Image:
	- Install Plugins:
		- `Docker API Plugin`
		- `Docker Commons Plugin`
		- `Docker Pipeline`
		- `Docker plugin`

	- Configure `Global Credentials` on `Jenkins`'s Dashboard:

- Configure `Jenkinsfile`

- Enter `docker-jk`:
```bash

$ chmod 666 /var/run/docker.sock
```

- Expected Outcomes: [Sample NodeJS DockerHub](https://hub.docker.com/repository/docker/pnguyen01/simple-to-do-nodejs-app)



## `Express` Web Server: *Super-simple-to-do-app*

## Container
- Create `Dockerfile`

### CI
- Create `Jenkinsfile`
	- Explain terms: [Explain terms](https://www.jenkins.io/doc/book/glossary/#:~:text=Agent,Build)

#### **Directory Structuring**
```yaml

```



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

### Troubleshooting:
- [Bash file - Permission Denied](https://stackoverflow.com/questions/46766121/permission-denied-error-jenkins-shell-script)
