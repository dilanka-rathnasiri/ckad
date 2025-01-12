| Command                                                           | Description                            |
|-------------------------------------------------------------------|----------------------------------------|
| `kubectl top node`                                                | Display resource usage of nodes        |
| `kubectl top pod`                                                 | Display resource usage of pods         |
| `kubectl get pod --selector {{label key}}={{label value}}`        | Get matching pods for the given labels |
| `kubectl get all`                                                 | Get all resources                      |
| `kubctl exec {{pod name}} -- {{command}}`                         | Execute a command inside a pod         |
| `kubectl describe pod -n kube-system kube-apiserver-controlplane` | View Kube API Server details           |
| `kubectl api-resources --namespaced={{true/false}}`               | List all resources                     |

* `--dry-run=client` option
    * Only preview the changes
    * Won't create resources