# Probes

## 3 types of probes

1. Liveness Probe
    * Determine when to restart the container
    * e.g., to detect deadlock
    * If a container repeatedly fails its readiness probe, kubelet restarts the container
    * Run during the pod's whole lifecycle
2. Readiness Probe
    * Determine when a container is ready to accept the traffic
    * e.g., determine a container's readiness when it has a long startup time
    * If a container fails its readiness probe, the pod will be removed from all the matching service endpoints
    * Run during the pod's whole lifecycle
3. Startup Probe
    * Determine whether the container's application has started
    * Useful to prevent kubelet killing the pod before the app starts
    * Startup probes disable liveness and readiness probes until the startup probe succeeds
    * Only run at the container's startup

> [!Important]
> * Liveness and readiness probes are work independently
> * So, they can overlap with each other

## Manifest file for defining probes


## 4 check mechanisms

1. exec
2. grpc
3. httpGet
4. tcpSocket

# References

* https://kubernetes.io/docs/concepts/configuration/liveness-readiness-startup-probes/
* https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/