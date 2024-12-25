# Deployment Strategies

* 2 strategies:
  * Recreate strategy
    * First, destroy all the old version's pods completely and then create the new version's pods
  * Rolling update strategy
    * Replace the old version's pods one by one with the new version's pods
    * Default strategy

* We can change the deployment strategy by changing `spec.strategy.type` in the deployment manifest file

## Important `kubectl` commands

| Command                                                 | Description                               |
|---------------------------------------------------------|-------------------------------------------|
| `kubectl rollout status deployment {{deployment name}}` | View rollout status                       |
| `kubectl rollout history deployment {{deploment name}}` | View rollout history                      |
| `kubectl rollout undo deployment {{deployment name}}`   | Undo to last previous deployment revision |

* `--record` option with `kubectl apply`  => for record the deployment revision
* If it is recorded => there will be a `change cause` in the `deployment history`
* When rollout to previous revision => Kubernetes creates a new revision from the previous revision and applies that revision to the deployment
* If the new revision's pods fail to start =>
  * Kubernetes proactively stops the update after replacing a few pods
  * Keep the running old revision's pods as it is without replacing with the new revision
  * Keep the already created new revision's pods with error