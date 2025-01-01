# Node Selectors

## Label A Node Using Kubectl

```shell
kubectl lable nodes {{node name}} {{lable key}}={{label value}}
```

e.g.,

```shell
kubectl label node node1 key1=value1
```

## View Labels of A Node Using Kubectl

```shell
kubectl get nodes {{node name}} --view-labels
```

e.g.,

```shell
kubectl get nodes node1 --view-labels
```

## Define Node Selector in Pod Manifest File

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
  nodeSelector:
    key1: value1
```