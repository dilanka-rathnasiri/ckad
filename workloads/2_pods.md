# Kubernetes Pods

## What are Kubernetes Objects?

* Kubernetes Objects are the foundational entities of Kubernetes
* We can consider Kubernetes objects as the building blocks of the Kubernetes
* Each Kubernetes object has its own specific attributes and responsibilities
* There are many types of Kubernetes objects
* The following are some of them
  * Pods
  * Deployments
  * Services

## What is a Pod?

* In Kubernetes, pods are the smallest deployable units of computing
* Kubernetes pod is one type of the Kubernetes objects
* A pod can contain one container or a group of tightly coupled containers
* So, Pods can be considered as abstractions that encapsulate one or more containers
* In most use cases, we use pods that contain a single container
* Pods are ephemeral and disposable

## Pod Manifest File

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

* In many use cases, we don't directly create and manipulate pods
* Instead, we’ll use workload resources like Deployments or ReplicaSets to manage pods at scale

## Multi-container pods

* Containers that are tightly coupled and required to work together can be encapsulated into a single pod
* These containers are automatically **co-located** and **co-scheduled** in the same physical or virtual machine
* So, this allows them to
  * communicate and coordinate with each other
  * share resources and dependencies
* e.g.:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
  - name: redis
    image: redis:latest
    ports:
    - containerPort: 6379
```

* There are several types of multi-container pods
* The following are 3 types used commonly
  * Sidecar Containers
  * Ambassador Containers
  * Adapter Containers

## Sidecar Containers

* Sidecar containers are the secondary containers that run along with the main application container within the same Pod
* They provide additional services or functionalities such as logging, monitoring, etc.
* In most cases, we don't directly manage sidecar containers
* Instead, Helm charts manage sidecar containers

## Init Container

* Init container is a container that runs before the main application containers of the pod
* They’re used for tasks required to be completed once before the application container startup
* Init containers are also regular containers
* But unlike regular containers,
  * Init containers always run to completion
  * Init containers run only at the pod startup
* A pod can have multiple init containers and they execute sequentially
* Init containers are specified within the `initContainers` section of the manifest file
* Init containers run sequentially in the order of `initContainers` section of the manifest file
* Only one init container runs at a time
* If an init container fails, it'll be restarted until it succeeds
* However, we can control the restart behavior by using `restartPolicy` of the pod
* If an init container fails, the whole pod will fail
* e.g.:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app.kubernetes.io/name: MyApp
spec:
  containers:
    - name: myapp-container
      image: busybox:1.28
      command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
    - name: init-myservice
      image: busybox:1.28
      command: ['sh', '-c', "until nslookup myservice.$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace).svc.cluster.local; do echo waiting for myservice; sleep 2; done"]
    - name: init-mydb
      image: busybox:1.28
      command: ['sh', '-c', "until nslookup mydb.$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace).svc.cluster.local; do echo waiting for mydb; sleep 2; done"]
```

* In the above example `init-myservice` and `init-mydb` are init containers
* Containers of the above manifest run in the following order,
  1. `init-myservice` init container starts first and runs to completion
  2. After successfully completing `init-myservice` init container, `init-mydb` init container starts and runs to the completion
  3. After successfully completing `init-mydb` init container, `myapp-container` container starts
