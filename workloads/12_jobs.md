# Jobs

## What is a Kubernetes Job?

* Kubernetes Jobs are for running one-off tasks that must reliably run to completion and stop

## Job manifest file

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: print-job
spec:
  completions: 3
  parallelism: 3
  ttlSecondsAfterFinished: 30
  template:
    spec:
      containers:
        - name: print-benz
          image: alpine:latest
          command: [ "/bin/sh", "-c" ]
          args:
            - |
              for i in $(seq 1 10); do
                echo "Benz"
                sleep 1
              done
      restartPolicy: Never
```

## Key fields in the Job manifest file

* `spec.completions`
    * Count of completion needed
    * Positive integer
    * Default: 1
    * If unset
        * Non-relevant for parallel jobs with work queue
        * For other job types, default value: 1
    * If 0 => no pod will run
    * Optional
* `spec.parallelism`
    * Count of parallel jobs
    * Non-negative integer
    * Default: 1
    * If unset => default value : 1
    * If 0 => Job is effectively paused
        * Manually need to change the `parallelism` value to resume the job
    * Optional
* `spec.template`
    * pods' template
    * Required
* `spec.restartPolicy`
    * How to handle the restarting of the pod
    * Only can be =>
        * `Never`
            * If the pod fails => whole pod will restart
        * `OnFailure`
            * If the pod fails => only failed containers inside the pod will restart
            * Pod stays on the node
    * Required
* `spec.ttlSecondsAfterFinished`
   * Time in seconds to clean the completed pods
   * Time starts for counting from the pod's completion
   * If 0 => clean up immediately after a pod's completion
   * If unset => never cleans up the completed pods
   * Optional
* `spec.backoffLimit`
  * Count of retries if pod fails
  * Default: 6
  * Optional

## Job types

* 3 job types
    * Non-parallel
        * Only run 1 pod
        * `spec.completions` must be 1 or unset
        * `spec.parallelism` must be 1 or unset
    * Parallel jobs with a fixed completion count
        * `spec.completions` must be a positive integer
        * `spec.parallelism` must be a non-negative integer or unset
    * Parallel job with a work queue
        * `spec.completions` must be unset
        * `sepc.parallelism` must be a non-negative integer or unset
        * Need an external queue
        * Each pod of the job runs independently of each other
