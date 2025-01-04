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
    name: pv-vol1
spec:
    accessModes:
      - ReadWriteOnce
    persistentVolumeReclaimPolicy: Retain
    capacity:
        storage: 1Gi
    awsElasticBlockStore:
        volumeID: {{volume-id}}
        fsType: ext4
```

## Key Fields In Persistent Volume Manifest File

* `spec.accessModes`
  * The access mode capabilities are different from one storage type to another
  * Valid values:
    * ReadWriteOncePod (**Most Commonly used**)
      * Only one pod can mount the volume as read and write
      * Even the pods in the same node can't mount the volume at the same time
    * ReadWriteOnce
      * Only one node can mount the volume as read and write
      * Pods are in the same node can mount the volume
    * ReadOnlyMany
      * Many nodes can mount the volumes as read-only
    * ReadWriteMany
      * Many nodes can mount the volume as read and write
* `spec.persistentVolumeReclaimPolicy`
  * What happens to the Persistent Volume after deleting the PVC
  * Values:
    * Retain:
      * Keep the Persistent Volume
      * Keep the data
    * Delete:
      * Delete the Persistent Volume
      * Also, delete the data
    * Recycle:
      * Keep the Persistent Volume
      * But delete the data

## Persistent Volume Claims

* PVC (Persistent Volume Claim) is used to mound a Persisted Volume to a Pod
* When PVCs look for a volume to bind:
  * (PVC storage size) <= (Volume storage size)
  * A PVC finds the most suitable volume to bind
  * If no suitable volume => PVC's status will be `pending`
  * If we want a specific volume to bound => We can use `labels` and `selectors`

## PVC Manifest File

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: car-pvc
spec:
  accessModes:
    - ReadWriteOnce # refer Persistent Volume's spec.accessModes
  resources:
    requests:
      storage: 500Mi
  selector:
    matchLabels:
      release: "stable"
```
