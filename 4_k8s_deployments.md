# Kubernetes Deployments

## What is a Kubernetes Deployment?

* A Deployment manages a stateless set of pods to run an application workload
* We can consider deployment as an abstraction layer for managing a set of pods
* We describe the desired state in a Deployment, then the Deployment Controller changes the actual state to the desired state at a controlled rate
* So, we indirectly manage ReplicaSets and Pods through the Deployments

## Deployment manifest file

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
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
        - name: nginx
          image: nginx
          ports:
            - containerPort: 80
```

* The above manifest file creates Deployment `my-deployment`
* When we create a Deployment, it automatically creates
  * A corresponding ReplicaSet managed by the Deployment
    * ReplicaSet's name started with `{{Deployment name}}-` prefix
    * e.g. `my-deployment-xxxxx`
  * Corresponding pods managed by the created ReplicaSet
    * Pods' name started with `{{ReplicaSet name}}-` prefix
    * e.g. `my-deployment-xxxxx-yyyy`
* Fields `.spec.replicas`, `.spec.template`, `.spec.template.metadata.labels`, and `.spec.selector.matchLabels` same as the ReplicaSet manifest file
