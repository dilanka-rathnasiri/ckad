# Resource Requirements

* Mainly 2 resource types
    * CPU
    * Memory
* Resource requirements can be applied for
    * Container wise
    * Pod wise (Not enabled by default)
* Resource requests value => Guaranteed resource value
    * Can exceed requests value
* Limit => Max resources value
    * Can't exceed limit
* (Request value) <= (Limit)
* By default, no requests values or limits for any object in Kubernetes
    * So, pod or container can consume all the resources of the cluster
* When only the limit is specified, but no requests values => requests value = limit
* When the pod or container exceeds the cpu limit => cpu starts to throttle
* When the pod or container exceeds the memory => Out of Memory (OOM) error
    * The pod or container will be killed
    * Reason will be `OOMKilled`

## Define Resource Requirements In Manifest File

Container wise:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: porsche
spec:
  containers:
    - name: taycan
      image: nginx:latest
      resources:
        requests:
          memory: "60Mi"
          cpu: "200m"
        limits:
          memory: "100Mi"
          cpu: "500m"
    - name: macan
      image: alpine:latest
      command:
        - sleep
        - inf
      resources:
        requests:
          memory: "80Mi"
          cpu: "250m"
        limits:
          memory: "200Mi"
          cpu: "700m"
```

Pod wise (Not enabled by default):

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: audi
spec:
  resources:
    limits:
      cpu: "1"
      memory: "200Mi"
    requests:
      cpu: "1"
      memory: "100Mi"
  containers:
    - name: r8
      image: nginx:latest
    - name: A7
      image: alpine:latest
      command:
        - sleep
        - inf
```

## Limit Ranges

* A Limit Range can,
    * Enforce min and max resource usage per a pod or container
    * Enforce min and max storage requests per a Persistent Volume Claim
    * Enforce a ratio between requests and limit for a resource
    * Set default requests and limit for resources
* Limit Ranges are namespace bounded
* Creating or updating LimitRange only affects new pods
    * It doesn't affect existing resources
* Limit Ranges block creating resources with violating the defined constraints

## Limit Range Manifest File

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-resource-constraint
  namespace: car-ns
spec:
  limits:
    - default: # default limit values
        cpu: 500m
      defaultRequest: # default requests values
        cpu: 500m
      max: # max limit
        cpu: "1"
      min: # min requests value
        cpu: 100m
      type: Container
```

## Key Fields In The Limit Range Manifest File

* `spec.limits.[].default` => Default value for limits of the resources
* `spec.limits.[].defaultRequest` => Default value for requests value of the resources
* `spec.limits.[].max` => max value for limits of the resources
    * Any resource's limit or requests value can't exceed this max value
* `spec.limits.[].min` => min value for requests value of the resources
    * Any resource's requests value can't be less than this min value

## Resource Quota

* Resource Quotas enforce constraints for aggregated resource consumption
* Can create constraints for
    * Compute Resources
    * Count of objects
* Resource Quotas are namespace bounded

## Resource Quota Manifest File

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: car-quota
  namespace: car-ns
spec:
  hard:
    requests.cpu: "1000m"
    requests.memory: "2000Mi"
    limits.cpu: "1500m"
    limits.memory: "2500Mi"
```

* Aggregated resource consumption can't exceed `car-quota` Resource Quota's hard limits
* If a limit is set => that limit should be set for all the resources
    * If `requests.memory` is set => `requests.memory` should be set for all the resources