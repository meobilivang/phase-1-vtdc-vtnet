
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

## IV. DEBUGGING:

### V. References



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

## 2. Start & Manage `Minikube`:

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

### 1. `Secret`:
- Create `secret-wordpress`:
```


### 2. `Deployment`

### 3. `Service`

### B2. DEPLOY:

- Check Running `Service`:
````bash
$  kubectl get services (<Service-name>)
````

**Note**
> `EXTERNAL-IP` always `<Pedning>` because `Minikube` exposes Services through `NodePort` only.

- Check IP of `wordpress`:
```bash
$ minikube service wordpress --url
```

# IV. DEBUGGING:

- Entering a `Container` in K8S with `bash`:
```bash
$ kubectl exec --stdin --tty <Pod-name> -- /bin/bash
```

- View logs:
```bash
$ kubectl logs <pod-name>
```

## 1. `Deployment in version "v1" cannot be handled as a Deployment...`

- Bug detected:
```bash
 env:
        - name: ALLOW_EMPTY_PASSWORD
          value: yes
```

- **Correct**: *Adding quotation marks* 
```bash
 env:
        - name: ALLOW_EMPTY_PASSWORD
          value: "yes"
```

# V. REFERENCES:

- [**kubectl** `apply` or `create`](https://www.digitalocean.com/community/tutorials/imperative-vs-declarative-kubernetes-management-a-digitalocean-comic)

- [Using Secrets](https://kubernetes.io/docs/concepts/configuration/secret/#secret-types)

- [Official Docs Example](https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/)

