| Task                                                       | Command                                                                       |
|------------------------------------------------------------|-------------------------------------------------------------------------------|
| Display resource usage of nodes                            | `kubectl top node`                                                            |
| Display resource usage of pods                             | `kubectl top pod`                                                             |
| Get matching pods for the given labels                     | `kubectl get pod --selector {{label key}}={{label value}}`                    |
| Get all resources                                          | `kubectl get all`                                                             |
| Execute a command inside a pod                             | `kubctl exec {{pod name}} -- {{command}}`                                     |
| View Kube API Server details                               | `kubectl describe pod -n kube-system kube-apiserver-controlplane`             |
| List all resources                                         | `kubectl api-resources --namespaced={{true/false}}`                           |
| Change Namespace in kubectl context                        | `kubectl config set-context --current --namespace {{namespace}}`              |
| Get previous instance of pod's log                         | `kubectl logs {{pod name}} -p` </br>`kubectl logs {{pod name}} --previous`    |
| Create a pod that is deleted automatically after completed | `kubectl run {{pod name}} --rm`                                               |
| List events with the given output type and given object    | `kubectl events --types=Warning,Normal --for={{object type}}/{{object name}}` |
| Delete object forcefully with 0 grace period               | `kubectl delete {{object type}} {{object name}} --force --grace-period=0`     |
| Create a job from a cron job                               | `kubectl create job {{job name}} --from=cj/{{cron job name}}`                 |

* `--dry-run=client` option
    * Only preview the changes
    * Won't create resources