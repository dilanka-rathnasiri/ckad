# RBAC: Role-Based Access Control

## Role Manifest File

````yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: uat
  name: dev-role
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
  resourceNames: ["mustang"]
````

* No Deny rules in roles
* Namespace bounded

## Key Fields Of Role Manifest File

* `rules.[].apiGroups`:
  * List of API groups like `core`, `apps`, `authentication.k8s.io`, `rbac.authorization.k8s.io`, and etc.
  * `""` => core api group
  * `"*"` => All api groups
* `rules.[].resources`:
  * List of resources like `pods`, `Deployments`, `Services`, `Secrets`, and etc.
  * `"*"` => All resources
* `rules.[].verbes`:
  * List of verbs like `get`, `list`, `update`, and etc.
  * `"*"` => All verbs
* `rules.[].resourceNames`:
  * List of allowed resources
  * `"*"` => All resourceNames
  * If unset => All resourceNames
  * Optional


## Role Binding Manifest File

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: admin-role-binding
  namespace: Deutschland
subjects:
- kind: User
  name: r8
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: audi
  apiGroup: rbac.authorization.k8s.io
```

* Namespace bounded
* Subject and role should be in the same namespace

## Key Fields In Role Binding Manifest File

* `subjects`:
  * Can be,
    * Users
    * Groups
    * Service Accounts
  * Can have multiple subjects
* `roleRef`:
  * Can be a Role or a Cluster Role

## Related Kubectl Commands

* `kubectl auth can-i {{verb}} {{resource}} --as {{user}} --namespace {{namespace}}`
  * Check access
  * If the user isn't given => Current user will be considered
  * If the namespace isn't given => Current namespace will be considered