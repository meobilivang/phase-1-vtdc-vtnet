
## TABLE OF CONTENTS
### I.

## II. Infrastructure & Architecture:

### A. Infrastructure:

### B. Architecture:
- Showing components of `K8S cluster`
````bash
$ kubectl get pods --namespace kube-system
````

### III. Step-by-step Guide

### IV. References



# II. STEP-BY-STEP GUIDE 

````bash
application/
│
│
│──secrets
│   ├── secret-mariadb.yml
│   └── secret-wordpress.yml
│
│
│──mariadb
│   	└── mariadb-deployment.yml
│
│
└──wordpress
    └── wordpress-deployment.yml

````

## A. `Minikube`:

## 1. INSTALL & DEPLOY `Minikube`:

## 2. Start & Manage `Minikube`
- Change user on Linux machine:

````bash
$ su minikube
````

- Start `Minukube`:
```bash
$ minikube start
``` 

- Stop `Minukube`:
```bash
$ minikube stop
``` 

## B. DEPLOYING `WordPress` & `MariaDB` with `Persistent Volumes`
### Create & Manage K8S Objects:

### 1. `Secret`

### 2. `Deployment`

### 3. `Service`

### B2. DEPLOY:


# REFERENCES:

- [**kubectl** `apply` or `create`](https://www.digitalocean.com/community/tutorials/imperative-vs-declarative-kubernetes-management-a-digitalocean-comic)

- [Using Secrets](https://kubernetes.io/docs/concepts/configuration/secret/#secret-types)

- [Official Docs Example](https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/)

