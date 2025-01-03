# Persistent Volume Claims

## Mount A Volumes To Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: car-pod
spec:
  containers:
    - name: app
      image: busybox:latest
      command: [ "ls", "/etc/app-config" ]
      volumeMounts:
        - name: config
          mountPath: "/etc/app-config"
          readOnly: true
  volumes:
    - name: config
      configMap:
        name: car-configmap
```
