# Node Affinity

## Node Affinity Types

* RequiredDuringSchedulingIgnoredDuringExecution
* PreferredDuringSchedulingIgnoredDuringExecution
* RequiredDuringSchedulingRequiredDuringExecution (Only planned, not available yet)

| Node Affinity Type                              | Scheduling | Execution |
|-------------------------------------------------|------------|-----------|
| RequiredDuringSchedulingIgnoredDuringExecution  | Required   | Ignored   |
| PreferredDuringSchedulingIgnoredDuringExecution | Preferred  | Ignored   |
| RequiredDuringSchedulingRequiredDuringExecution | Required   | Required  |

## Define Node Affinity In Manifest File

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx
      image: nginx:latest
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: key1
                operator: In
                values:
                  - value1
                  - value2
```

## Key Fields in Manifest File

* We can use the following values for the affinity operator
    * `In`
    * `NotIn`
    * `Exists`
    * `DoesNotExist`
