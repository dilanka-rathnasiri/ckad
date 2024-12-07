# Kubernetes ReplicaSet

## What Is a ReplicaSet?

* A ReplicaSet is a Kubernetes object that ensures a specified number of pods of a given template are running at any
  given time
* It guarantees the availability of a specified number of identical pods
* Most of the time, we don't directly manage ReplicaSets
* Instead, we use Deployments for managing ReplicaSet

## ReplicaSet Manifest File

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-container
          image: nginx
          ports:
            - containerPort: 80
```

## Key Fields in the ReplicaSet Manifest File

* `.spec.replicas`:
  * Number of pods managed by the ReplicaSet
  * In the above ReplicaSet, we have 3 pods
  * If we don't specify a value, then the default value is 1
* `.spec.template`:
  * The pod template used to create the ReplicaSet’s pods
* `.spec.template.metadata.labels`:
  * labels of the pods created from the pod template
  * These labels must match `.spec.selector.matchLabels` labels
  * In the above ReplicaSet, the labels of the pods are `app: my-app`
* `.spec.selector.matchLabels`:
  * Replicaset identifies the pods required to be managed by the ReplicaSet from these labels
  * ReplicaSet will manage pods with matching labels (even if not created by the ReplicaSet)
  * e.g., if 2 pods with the label `app: my-app` already exist, the ReplicaSet will create only 1 more pod but manage all 3 pods
  * ReplicaSets can also use advanced label selectors like `matchExpressions` instead of `matchLabels`

## How a ReplicaSet Links to Its Pods

* A ReplicaSet links to its pods via the `metadata.ownerReferences` field in the pod's metadata.
* Field `metadata.ownerReferences` specifies the owner of the object
* This field specifies the ReplicaSet `my-replicaset` as the owner
* Here's an example output YAML of a pod managed by `my-replicaset`

```yaml
apiVersion: v1
kind: Pod
metadata:
  generateName: my-replicaset-
  labels:
    app: my-app
  name: my-replicaset-dxzzg
  namespace: default
  ownerReferences:
    - apiVersion: apps/v1
      blockOwnerDeletion: true
      controller: true
      kind: ReplicaSet
      name: my-replicaset
      uid: a93e48a6-9ec6-46e6-b92f-c61ccd9a2d40
  resourceVersion: "472"
  uid: 6cf20e31-a73a-4846-ad8d-a4b41e7711ac
spec:
  containers:
    - image: nginx
      imagePullPolicy: Always
      name: my-container
      ports:
        - containerPort: 80
          protocol: TCP
```

## Replicaset vs. Replication Controller

* ReplicaSet is the next generation of Replication Controller
