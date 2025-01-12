# Namespaces

## What is a Namespace?

* Namespace is a logically isolated group of resources with in a single cluster
* Names of resources in a namespace must be unique within the namespace
* But the same name can exist for resources in different namespaces
* Most Kubernetes resources are in some namespaces
  * eg: pods, services, deployments, etc.
* But some Kubernetes resources aren't in any namespace
  * eg: nodes, persistent volumes, namespace, etc.
* So, there are 2 types of Kubernetes objects:
  * Namespace scoped resources
  * Cluster scoped resources

## Initial Namespaces

Kubernetes starts with 4 initial namespaces

* default -> This is the default namespace
* kube-node-lease
* kube-public 
* kube-system -> Objects created by the Kubernetes system are in this namespace

* The best practice is to avoid using the default namespace
* Instead, create a new namespace and use it
* Also, we shouldn't create namespaces with prefix `kube-`
* Because it's reserved for Kubernetes system namespaces

## Namespace manifest file

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace
```

> [!WARNING] 
> Deleting a namespace deletes all the resources in the namespace 
