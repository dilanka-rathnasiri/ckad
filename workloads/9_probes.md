# Probes

* Probes are configured for containers

## 3 types of probes

1. Liveness Probe
    * Determine when to restart the container
    * e.g., to detect deadlock
    * If a container repeatedly fails its liveness probe, kubelet restarts the container
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

## Manifest file for probes

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx:latest
      livenessProbe:
        httpGet:
          port: 80
          path: /
        initialDelaySeconds: 5
        periodSeconds: 5
      readinessProbe:
        httpGet:
          port: 80
          path: /
        initialDelaySeconds: 5
        periodSeconds: 5
      startupProbe:
        httpGet:
          port: 80
          path: /
        initialDelaySeconds: 5
        periodSeconds: 5
```

## Key fields in the manifest file

* `livenessProbe`
    * Define liveness probe for the container
* `readinessProbe`
    * Define readiness probe for the container
* `startupProbe`
    * Define startup probe for the container
* `initialDelaySeconds`
    * Initial probe's delay in seconds
    * Default: 0
    * Minimum: 0
    * if (`periodSeconds` > `initialDelaySeconds`) => `initialDelaySeconds` will be ignored
* `periodSeconds`
    * Time interval in seconds for how often to perform the probe
    * Default: 10
    * Minimum: 1
    * If the pod isn't ready => Readiness probe will be performed with less time interval than defined `periodSeconds` => This makes the pod ready faster
* `timeoutSeconds`
    * Probe's timeout in seconds
        * If `httpGet` probe => request timeout in seconds
    * Default: 1
    * Minimum: 1
* `successThreshold`
    * Minimum consecutive success for considering the probe as successful after having failed
    * Default: 1
    * Minimum: 1
    * for `livenessProbe` => must be 1
    * for `startupProbe` => must be 1
* `failureThreshold`
    * Consecutive failures for considering the probe as failed
    * Default: 3
    * Minimum: 1
    * If liveness or startup probe fails => kubelet consider the pod as unhealthy => kubelet restart the pod
    * If the readiness probe fails => kubelet set the pod's `ready` condition as `false`
* `terminationGracePeriodSeconds`
    * Grace period before the pod's termination
    * Time between triggering the shutdown and forcefully stop the container
    * that means: 1 <= t <= `terminationGracePeriodSeconds`
    * Default: inherit from the pod level `terminationGracePeriodSeconds`
        * If pod level isn't defined => default: 30
    * Minimum => 1

## 4 check mechanisms

1. exec

```yaml
livenessProbe:
  exec:
    command:
      - cat
      - /tmp/healthy
```

2. grpc

```yaml
livenessProbe:
  grpc:
    port: 2379
```

3. httpGet

```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 8080
    httpHeaders:
      - name: Custom-Header
        value: ItsAlive
```

4. tcpSocket

```yaml
livenessProbe:
  tcpSocket:
    port: 8080
```
