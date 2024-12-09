# Pod lifecycle

## Pod phases

1. Pending
    * Kubernetes cluster has accepted the pod
    * But one or more containers in the pod haven't been set up
2. Running
    * Pod has been bound to a node
    * All containers have been created
    * At least one container is still running
3. Succeeded
    * All containers in the pod have successfully exited
    * No one has restarted
4. Failed
    * All the containers have terminated
    * But at least one container has terminated with failure
5. Unknown
    * Pod state can't be obtained
    * Typically, occurs due to a communication error

> [!Important]
> `Status` field in the `kubectl` output isn't pod status
> It shows more user-friendly output

## Container states

1. Waiting
    * Set up time of the container
    * e.g., pulling the image
2. Running
3. Terminated
    * Container exited
    * Can be successfully completed or failed

## Pod conditions

* Array of boolean variables

1. PodScheduled
    * Pod has been scheduled to a node
2. PodReadyToStartContainers
    * Pod's sandbox has been created and networking configured
3. ContainersReady
    * Pod's all containers are ready
4. Initialized
    * Pod's all init containers have successfully completed
5. Ready
   * Pod is ready
   * Pod is added to the relevant services' load balancing pools

## Container restart policies

* Always
    * Restart the container after any termination for both successful and failure
* OnFailure
   * Restart the container only if the container terminated with failure
* Never
  * Never restart the container after any termination