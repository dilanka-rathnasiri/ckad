# CronJobs

* CronJob is a scheduled job with a cron expression
* Short name: `cj`
* Cron job will create seperate job for each shedule
  * If the cron expression is "*/1 * * * *"
  * Then for each and every minute there will be a sheduled execution
  * That means in each schedule there will be a newly created job

## CronJob manifest file

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: print-cronjob
spec:
  startingDeadlineSeconds: 7 # if the job doesn't start after 7s,job will miss
  schedule: "*/1 * * * *"
  jobTemplate:
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

## Key fields in the CronJob manifest file

* `spec.schedule`
  * Cron expression for scheduling
  * Required

```markdown
  ┌───────────── minute (0 - 59)
  │ ┌───────────── hour (0 - 23)
  │ │ ┌───────────── day of the month (1 - 31)
  │ │ │ ┌───────────── month (1 - 12)
  │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday)
  │ │ │ │ │                                   OR sun, mon, tue, wed, thu, fri, sat
  │ │ │ │ │ 
  │ │ │ │ │
  * * * * *
```
* `spec.jobTemplate`
  * Job's template
  * Only required field `spec` of the job manifest file
  * Fields `apiVersion`, `kind`, `metadata` are excluded
* `spec.startingDeadlineSeconds`
  * Deadline for the starting time from the job after it schedules
    * If the job can't start within the `startingDeadlineSeconds` time period
      * That job will be skipped
      * That job won't run after the `startingDeadlineSeconds`
      * That job won't be considered as failed
      * But that job simply won't start
      * The next job for the next trigger will run in the next scheduled time
