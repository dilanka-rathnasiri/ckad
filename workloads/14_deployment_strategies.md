# Deployment Strategies

* We can change the deployment strategy by changing `spec.strategy.type` in the deployment manifest file
* 2 strategies:
    * Recreate strategy
        * First, destroy all the old version's pods completely and then create the new version's pods
    * Rolling update strategy
        * Replace the old version's pods one by one with the new version's pods
        * Default strategy
        * Max unavailable
            * The maximum number of pods that can be unavailable during the update process
            * In manifest file => `spec.strategy.rollingUpdate.maxUnavailable`
            * Can be percentage or a non-negative integer
            * Default => 25%
            * Optional field
            * If note set => default: 25%
        * Max surge
            * The maximum number of pods that can be created over the desired pods count
            * In manifest file => `spec.strategy.rollingUpdate.maxSurge`
            * Can be percentage or a non-negative integer
            * Default => 25%
            * Optional field
            * If not set => default: 25%
        * Only one can be 0 from maxUnavailable and maxSurge
        * Also, both values can be positive integers or percentages

## Important `kubectl` commands

| Command                                                 | Description                               |
|---------------------------------------------------------|-------------------------------------------|
| `kubectl rollout status deployment {{deployment name}}` | View rollout status                       |
| `kubectl rollout history deployment {{deploment name}}` | View rollout history                      |
| `kubectl rollout undo deployment {{deployment name}}`   | Undo to last previous deployment revision |

* `--record` option with `kubectl apply`  => for record the deployment revision
* If it is recorded => there will be a `change cause` in the `deployment history`
* When rollout to previous revision => Kubernetes creates a new revision from the previous revision and applies that
  revision to the deployment
* If the new revision's pods fail to start =>
    * Kubernetes proactively stops the update after replacing a few pods
    * Keep the running old revision's pods as it is without replacing with the new revision
    * Keep the already created new revision's pods with error

## _Additional Deployment Strategies [Not natively supported in Kubernetes]_

#### Blue/Green Deployment

* Old version => Blue
* New version => Green

Process:

1. Only the blue version exists
    * Still no green version
2. Deploy the green version
    * Only deploy
    * Don't direct the traffic to the green version yet
3. Complete all the testing for the green version
4. If tests pass => direct all the traffic to the green version
5. Delete the blue version

#### Canary Deployment

1. Only the old version exists
2. Deploy the new version
3. Route a small percentage of the traffic to the new version
4. Run tests for the new version
5. If tests pass => Replace the old version with the new version
    * Can use rolling update for replacing