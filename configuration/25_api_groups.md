# API Groups

## Directly Call Kube API Server

```shell
curl https://kube-master-ip:6443 --key {{access key}} --cert {{certificate}} --cacert {{ca certificate}}
```

## Different API Groups

* `/metrics`
* `/healthz`
* `/version`
* `/api` -> Core group
* `/apis` -> Named group
* `/logs`

```mermaid
graph TD
    A["api (core)"] --> B["v1"]
    
    subgraph C["resources"]
        C1["pods"]
        C2["configmaps"]
        C3["services"]
        C4["..."]
    end
    
    B --> C1
    B --> C2
    B --> C3
    B --> C4
    
    subgraph D["verbs"]
        D1["get"]
        D2["create"]
        D3["delete"]
        D4["update"]
        D5["..."]
    end
    
    C1 --> D1
    C1 --> D2
    C1 --> D3
    C1 --> D4
    C1 --> D5
```

```mermaid
graph TD
    E["apis (named)"]
    
    subgraph F["groups"]
        F1["apps"]
        F2["authentication.k8s.io"]
        F3["authorization.k8s.io"]
        F4["..."]
    end
    
    E --> F1
    E --> F2
    E --> F3
    E --> F4

    F1 --> G["v1"]
    
    subgraph H["resources"]
        H1["deployments"]
        H2["replicasets"]
        H3["statefulsets"]
        H4["..."]
    end
    
    G --> H1
    G --> H2
    G --> H3
    G --> H4
    
    subgraph I["verbs"]
        I1["get"]
        I2["create"]
        I3["delete"]
        I4["update"]
        I5["..."]
    end
    
    H1 --> I1
    H1 --> I2
    H1 --> I3
    H1 --> I4
    H1 --> I5
```

## Access Kube API Server Through Kubectl Proxy

```shell
# start kubectl proxy
kubectl proxy

# call kube api server using curl
curl http://127.0.0.1:8001
```

> Note:
> kube proxy != kubectl proxy