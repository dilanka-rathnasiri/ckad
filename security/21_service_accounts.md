## Service Accounts

* Service accounts provide identities for non-humans entities
* **In previous versions:**
    * Tokens are stored in Secrets
* **In newer versions:**
    * When creating a new Service Account, a token won't be created
        * No tokens even for the default service accounts
        * We can manually create a token using kubectl with the following command
          ```shell
          kubectl create token {{service account name}}
          ```
        * The above command only creates a token and prints on the terminal
        * Token isn't stored in any place
    * Since no token => Tokens aren't stored in secrets
    * **TokenRequest API** generates a time-based token for the pod
        * That generated token for the pod has
            * Expiration time
            * Audience
* When pod uses a Service Account => `/var/run/secrets/kubernetes.io/serviceaccount` directory will be mounted to the
  pod
    * This is only a projected directory
    * `TokenRequest API` manage this projected directory
    * The following files will be in there
        * `ca.crt`
        * `namespace`
        * `token` => This is the JWT token

## Create Service Accounts Using Kubectl

```shell
kubectl create serviceaccocunt {{service account name}}
```

## Service Account Manifest file

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: build-robot
```

## Default Service Accounts

* Each namespace has its own default Service Account
* The default Service Accounts have very limited permissions
* If we don't assign any Service Account to an object => Then default Service Account will be used

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
