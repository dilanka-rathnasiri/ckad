# Taints and Tolerations

* Taints are applied to nodes
* Taints repel a set of pods from a node
* Tolerations are applied to pods
* Tolerations allow the scheduler to schedule pods with matching taints
* Taints and tolerations don't ensure pod placement in a specific node
* Taints and tolerations can only block the unwanted pods from placing on the node
* Kubernetes master node has a taint for preventing placing pods on the master node

## Taint Effects

* NoExecute
    * If the already running pod hasn't matched toleration => That pod will be evicted immediately from the node
    * If the new pod hasn't matched toleration => That new pod won't be scheduled on the node
* NoSchedule
    * If the new pod hasn't matched toleration => That new pod won't be scheduled on the node
    * If the already running pod hasn't matched toleration => That pod won't be evicted from the node
        * The already running pod without matched toleration will remain on the node
* PreferNoSchedule
    * Soft version of NoSchedule
    * If the new pod hasn't matched toleration => Control plane tries to avoid placing the new pod on the node with
      taint
        * But there is no guarantee
    * If the already running pod hasn't matched toleration => That pod won't be evicted from the node

## Add Taint To A Node Using Kubectl

```shell
kubectl taint nodes {{node name}} {{taint key}}={{taint value}}:{{taint effect}}
```

e.g.,

```shell
kubectl taint nodes node1 key1=value1:NoSchedule
```

## Remove Taint From A Node Using Kubectl

```shell
kubectl taint nodes {{node name}} {{taint key}}={{taint value}}:{{taint effect}}-
```

e.g.,

```shell
kubectl taint nodes node1 key1=value1:NoSchedule-
```

## Define Toleration For Pods In Manifest File

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
    - name: nginx
      image: nginx:latest
  tolerations:
    - key: "key1"
      operator: "Equal"
      value: "value1"
      effect: "NoSchedule"
```

## Key Fields In Manifest File

* `spec.toleratios`
    * Toleration for the pod
* `spec.tolerations.[].key`
    * Key of the taint
* `spec.tolerations.[].operator`
    * Can be:
        * `Equal`
        * `Exists`
    * Default: `Equal`
* `spec.tolerations.[].value`
    * Value of the taint
    * Only required for operator `Equal`
* `spec.tolerations.[].effect`
    * Taint effect
    * Can be:
        * `NoExecute`
        * `NoSchedule`
        * `PreferNoSchedule`
    * If unset => Enable for all taint effects
    * If set => Must be matched the relevant taint effect of the node
        * If nodes taint is `NoSchedule` => Pod's toleration should be `NoSchedule`
