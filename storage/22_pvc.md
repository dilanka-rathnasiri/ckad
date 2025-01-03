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
## Persistent Volume Manifest File

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: block-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  volumeMode: Block
  persistentVolumeReclaimPolicy: Retain
  fc:
    targetWWNs: ["50060e801049cfd1"]
    lun: 0
    readOnly: false
```

## Access Modes in Persistent Volumes

* 4 types
  * ReadWriteOnce
    * Only one node can mount the volume as read and write
    * Pods are in the same node can access the volume
  * ReadOnlyMany
    * Many nodes can mount the volumes as read-only
  * ReadWriteMany
    * Many nodes can mount the volume as read and write
  * ReadWriteOncePod
    * Only one pod can mount the volume as read and write
    * Even the pods in the same node can't mount the volume at the same time
* The access mode capabilities are different from one storage type to another
