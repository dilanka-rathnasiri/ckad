# KubeConfig

* Credentials are stored in the KubeConfig file
* Kind: `Config` (Config != ConfigMap)
* Default location: `$HOME/.kube/config`
* Has main 3 sections
    * `clusters`
    * `contexts`
        * `{{user}}@{{cluster}}`
    * `users`

## KubeConfig File Format

```yaml
apiVersion: v1
current-context: developer&dev-cluster
kind: Config
clusters:
  - name: dev-cluster
    cluster:
      certificate-authority-data: 77777AAAADDDDRRRRR1111177777AAAAADDD
      server: https://aabbcc.dar.com
  - name: prod-cluster
    cluster:
      certificate-authority: /var/run/secrets/kubernetes.io/ca.crt
      server: https://abcd.dar.com
contexts:
  - name: developer&dev-cluster
    context:
      cluster: dev-cluster
      user: developer
  - name: admin&prod-cluster
    context:
      cluster: prod-cluster
      user: admin
users:
  - name: developer
    user:
        client-certificate: cert-file-1
        client-key: key-file-1
  - name: admin
    user:
      client-certificate: cert-file-2
      client-key: key-file-2
```

## Key Fields In KubeConfig File

* `clusters`
  * List of clusters
  * A cluster item contains:
    * `clusters.[].cluster.server`: Cluster's url
    * `clusters.[].cluster.certificate-authority-data`: Base64 encoded certificate body
    * `clusters.[].cluster.certificate-authority`: Path of the certificate
  * Only one of `clusters.[].cluster.certificate-authority-data` or `clusters.[].cluster.certificate-authority` is required
* `users`: List of users
* `contexts`
  * List of `{{user}}&{{cluster}}`
  * A context item contains:
    * `contexts.[].context.cluster`
    * `contexts.[].context.user`
    * `contexts.[].context.namespace` (Optional)
* `current-context`:
  * Context currently in used
  * Must be from `contexts`

## View KubeConfig Using Kubectl

```shell
kubectl config view
```

## Use Custom KubeConfig File With Kubectl

* Use `--kubeconfig` option

## Change Current Context Using Kubectl

```shell
kubectl config use-context {{context name}}
```

## Change Default KubeConfig File Of Kubectl

```shell
export KUBECONFIG="{{file path}}" 
```
