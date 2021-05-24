# WEEK-4 PRACTICE DOCUMENTATION
# PaaS - Platform as A Service w/ **`Kubernetes`**
---

## **Author:** *Julian (Phong) Ng.* 
**Date of issue**: *May 25th 2021*

> Welcome back! This is the documentation for my second training project at **Viettel Network**. Enjoy ur time :smile_cat:. Feel free to hit me up if any edition is needed!

---

## TABLE OF CONTENTS
### I. Overview

### II. Prequisite:

### III. Architecture:

### IV. Step-by-step Guide

### V. Debugging:

### VI. References


# **I. OVERVIEW:**

## **1. `KUBERNETES`**

## **2. `MINIKUBE`**

# **II. PREREQUISITE:**

## **A. Knowledge:**


## **B. Infrastructure:**

- Showing components of `K8S cluster`
````bash
$ kubectl get pods --namespace kube-system
````

# **III. STEP-BY-STEP GUIDE** 

**Suggested Directories Layout**
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

## **A. `Minikube`:**

## **1. INSTALL & DEPLOY `Minikube`:**

## **2. Start & Manage `Minikube`:**

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

## **B. DEPLOYING `WordPress` & `MariaDB` with `Persistent Volumes`**
### **B1. Create & Manage K8S Objects:**

### 1. `Secret`:
 
**Note**: *All `data` fields in `Secret` should be `base64` encoded*
	- **db_user**: pnguyen
	- **db_name**: wordpress_k8s
	- **db_password**: 12345678

- Encode to `base64`: *Repeat for data fields*
````bash
$ echo "<input>" | base64
````

- Create `secret-mariadb`:
```bash

$ vi secrets/secret-mariadb.yml

----
apiVersion: 1
kind: Secret
metadata:
  name: secret-mariadb
type: Opaque
data:
  db_user: cG5ndXllbgo=
  db_name: d29yZHByZXNzX2s4cw==
  db_password: MTIzNDU2Nzg=
```

- Create `secret-wordpress`:
```bash

$ vi secrets/secret-wordpress.yml

-----
apiVersion: 1
kind: Secret
metadata:
  name: secret-wordpress
type: Opaque
data:
  db_user: cG5ndXllbgo=
  db_name: d29yZHByZXNzX2s4cw==
  db_password: MTIzNDU2Nzg=
```

### 2. `Service`:

- `Service` for `mariadb`:

```bash

apiVersion: v1
kind: Service
metadata:
  name: wordpress-mariadb
  labels:
    app: wordpress-k8s
spec:
  ports:
    - port: 3306
  selector:
    app: wordpress-k8s
    tier: mariadb
  clusterIP: None

```

- Create `secret-wordpress`:
```bash

$ vi secrets/secret-wordpress.yml


```


### 3. `Deployment`:

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

