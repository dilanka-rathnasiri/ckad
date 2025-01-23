# Network Policies

## Network Policy Manifest File

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: cloud-network-policy
  namespace: cloud-system
spec:
  podSelector: # pods needed to be applied are selected from this
    matchLabels:
      app: db
  policyTypes:
  - Ingress
  - Egress
  ingress: # `Ingress` should be in `spec.policyTypes` fields for enabling
  - from: # if one rule item match => allow [as OR operator]
    - ipBlock:
        cidr: 172.17.0.0/16
        except:
        - 172.17.1.0/24
    - namespaceSelector: # both rules as the one list item => both should be matched to allow [as AND operator]
        matchLabels:
          department: db-team
      podSelector:
        matchLabels:
          role: db-admin
    - podSelector:
        matchLabels:
          role: developer
    ports:
    - protocol: TCP
      port: 6379
    - protocol: TCP
      port: 3000
  egress: # `Egress` should be in `spec.policyTypes` fields for enabling
  - to:
    - ipBlock:
        cidr: 10.0.0.0/24
    ports:
    - protocol: TCP
      port: 5978
```

## Key Fields In Network Policy Manifest File

* `spec.policyTypes`
  * Values: `Ingress`, `Egress`
  * If a value is specified and no rules are specified:
    * All requested will be denied
  * If no value => Allows all requests
  * This field is optional

## Network Policies in Minikube

* Minikube doesn't support network policies by default
* We have to use Calico for that
* Use the following command to use Calico in Minikube
```shell
minikube start --cni calico
```
