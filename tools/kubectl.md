| Task                                   | Command                                                           |
|----------------------------------------|-------------------------------------------------------------------|
| Display resource usage of nodes        | `kubectl top node`                                                |
| Display resource usage of pods         | `kubectl top pod`                                                 |
| Get matching pods for the given labels | `kubectl get pod --selector {{label key}}={{label value}}`        |
| Get all resources                      | `kubectl get all`                                                 |
| Execute a command inside a pod         | `kubctl exec {{pod name}} -- {{command}}`                         |
| View Kube API Server details           | `kubectl describe pod -n kube-system kube-apiserver-controlplane` |
| List all resources                     | `kubectl api-resources --namespaced={{true/false}}`               |
| Change Namespace in kubectl context    | `kubectl config set-context --current --namespace {{namespace}}`  |

* `--dry-run=client` option
    * Only preview the changes
    * Won't create resources