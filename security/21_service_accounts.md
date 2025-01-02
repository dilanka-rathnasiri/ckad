## Service Accounts

## What is a Service Account?

* Service accounts provide identities for non-humans entities

## Create Service Accounts Using Kubectl

```shell
kubectl creeate serviceaccocunt {{service account name}}
```

## Service Account Manifest file

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: build-robot
```
* **In newer versions of Kubernetes, When creating a new Service Account, a token won't be created**
* But we can manually create a token for the Service Account
  * This token has an expiration time and audience
* We can create a token using kubectl with the following command

```shell
kubectl create {{token name}} {{service account name}}
```

## Default Service Accounts

* There is a default Service Account for each Namespace
* The default Service aAccounts have very limited permissions
* If we don't assign any Service Account to an object => Then default Service Account will be used
* Default Service Account has a token
  * This token stores as a mutable Secret
  * This Token has an expiration time and audience
  * *TokenRequest API* generate this token
* If a pod uses default Service Account => `/var/run/secrets/kubernetes.io/serviceaccount` directory will be mounted to the pod
  * This is only a projected directory
  * `TokenRequest API` manage this projected directory
  * The following files will be in there
    * `ca.crt`
    * `namespace`
    * `token` => This is the JWT token

## Non-Expiring Token Secret Manifest File For A Service Account (Deprecated)

```yaml
apiVersion: v1
kind: Secret
type: kubernetes.io/service-account-token
metadata:
  name: {{secret name}}
  annotations:
    kubernetes.io/service-account.name: {{service account name}}
```

* Here we have used
  * Type: `kubernetes.io/service-account-token`
  * `kubernetes.io/service-account.name: {{service account name}}` annotation
