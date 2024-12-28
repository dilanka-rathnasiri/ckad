# Services

## Types of Services

* Mainly 3 types
    * NodePort
        * Expose the service to outside the cluster
        * Provide static port
        * The specified port is allocated from each node of the cluster
        * 30000 <= static port <= 32767
        * We can access the service by,
            * {{cluster ip}}**:**{{allocated port}}
            * {{node ip}}**:**{{allocated port}}
    * ClusterIp
    * LoadBalancer

## Service Manifest File

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  selector:
    app: car-app
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30007
```

## Key fields in the Service Manifest File

* `type`
    * Type of the service
    * Optional
* `selector`
    * Identify the target pods for the service
    * Required
* `port.[].port`
    * Service port
    * Required
* `port.[].targetPort`
    * Pod's port
    * Optional
    * If it doesn't specify => same as `port.[].port`
* `port.[].nodePort`
    * Port specify for the outside of the cluster
    * Only available for NodePort type
    * Optional