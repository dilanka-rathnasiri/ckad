# Resource Requirements

* Mainly 2 resource types
    * CPU
    * Memory
* Resource requirements can be applied for
    * Container wise
    * Pod wise (Not enabled by default)
* Resource requested value => Guaranteed resource value
    * Can exceed request value
* Limit => Max resources value
    * Can't exceed limit
* By default, no requested values or limits for any pod in Kubernetes
    * So, pod or container can consume all the resources of the cluster
* When only the limit is specified, but no requested values => requested value = limit
* When the pod or container exceeds the cpu limit => cpu starts to throttle
* When the pod or container exceeds the memory => Out of Memory (OOM) error
    * The pod or container will be killed

## Define Resource Requirements In The Manifest File

Container wise:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: frontend
spec:
  containers:
  - name: app
    image: images.my-company.example/app:v4
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
  - name: log-aggregator
    image: images.my-company.example/log-aggregator:v6
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```

Pod wise (Not enabled by default):

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-resources-demo
  namespace: pod-resources-example
spec:
  resources:
    limits:
      cpu: "1"
      memory: "200Mi"
    requests:
      cpu: "1"
      memory: "100Mi"
  containers:
  - name: pod-resources-demo-ctr-1
    image: nginx
  - name: pod-resources-demo-ctr-2
    image: fedora
    command:
    - sleep
    - inf
```

## LimitRange

* namespace bounded
* Updating LimitRange only affects new pods
    * Don't affect existing resources

## Resource Quota

* Apply for the namespace