# Kubernetes Cluster Architecture

![App Platorm](https://kubernetes.io/images/docs/kubernetes-cluster-architecture.svg)
*Figure 1: Basic Kubernetes cluster architecture*

* Figure 1 shows the architecture of a basic Kubernetes cluster
* More components can be added to the cluster depending on the use cases
* There are 2 main parts in the Kubernetes cluster.
  - Control plane
  - Worker nodes

## Control plane
* Control plane controls the cluster
* It acts as the brain of the cluster
* It makes decisions about the cluster
* It detects and responds to cluster events
* Components of the control plane:
  * kube-apiserver
  * etcd
  * kube-scheduler
  * kube-controller-manager
  * cloud-controller-manager

## Worker nodes
* Worker nodes do the actual workload of applications
* They act as the muscles of the cluster
* There will be one or more worker nodes
* Components of the worker node:
    * kubelet
    * kube-proxy
    * Container runtime
* Each worker node consists of the above node components

## Control plane components

### kube-apiserver

* Kube-apiserver exposes Kubernetes APIs for interacting with the Kubernetes cluster
* It acts as the front-end of the control plane
* `kube-apiserver` can be scaled horizontally.
* We can access Kubernetes APIs via:
  * REST API calls
  * CLI tools such as kubectl, kubeadm


### etcd

* `Etcd` is a key value store for all cluster data
* It is an etcd data store
* Highly available, reliable, and distributed
* Data stored in the etcd:
  * Cluster configuration data
  * Information about Kubernetes objects' states

### kube-scheduler

* `Kube-scheduler` maintains the health of the cluster
* It watches the pods
* It schedules pods to an appropriate node
* Various factors are considered when scheduling
* The following are some of them:
  * individual and collective resource requirements
  * hardware/software/policy constraints
  * affinity and anti-affinity specifications

### kube-controller-manager

* `Kube-controller-manager` consists of multiple controllers
* These controllers make the decisions about the cluster and maintain the cluster
* Logically, each controller can be considered as a separate process
* To reduce the complexity, all the controller processes are compiled into a single binary and run as a single process
* The following are some of them:
  * Node Controller: Noticing and responding when nodes go down
  * Job Controller: Watches the job objects and executes them
  * EndpointSlice Controller: Populate EndpointSlice objects
  * ServiceAccount Controller: Create default ServiceAccounts for new namespaces

### cloud-controller-manager

* `Cloud-controller-manager` contains cloud service provider's (e.g., AWS, GCP, etc.) specific controller logics
* It links the cluster with the cloud provider's APIs 
* Similar to the kube-controller-manager, cloud-controller-manager consists of multiple control logics compiled into a single binary and run as a single process 
* If the Kubernetes cluster is on-premise or in a learning environment (e.g., Minikube, kind, etc.), there's no cloud-controller-manager

## Worker nodes components

### Container Runtime

* Container runtime empowers Kubernetes to run containers effectively
* It manages the execution and lifecycle of containers 
* Kubernetes supports container runtimes that are any implementation of Kubernetes CRI (Container Runtime Interface)
* The following are some of the Container runtimes:
  * Docker
  * Containerd
  * CRI-O

### kubelet

* `Kubelet` is an application that communicates with the control plane
* It acts as an agent of the control plane
* It executes the decisions made by the control plane
* It takes a set of PodSpecs and ensures that the containers described in the PodSpecs are running and healthy

### kube-proxy

* `Kube-proxy` is a network proxy for facilitating Kubernetes networking services
* It handles communication inside or outside the cluster 
* If there is a network plugin that has similar functionalities to the kube-proxy, kube-proxy won't be necessary
