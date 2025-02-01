# Exam Tips

## Details

* Time: 2 hours
* Questions: 19
* So, time management is critical

## Tips

* Try to do the questions with the highest marks first
* Try to do the easiest questions first
* No need of answering questions sequentially
* Note down the difficult questions with their marks in the given notepad
* Refer Kubernetes Documentation
* Read the instructions on the paper for coping from the Kubernetes Documentation
* Understand:
  * linux file systems
  * ssh
  * volume mounts
* Use **imperative commands** as much as possible because there's no time to edit YAML files
  * Use `kubectl run`
* Use the following bash aliases
  ```shell
  export ns=default
  alias k='kubectl -n $ns'
  alias kdr= 'kubectl -n $ns -o yaml --dry-run'
  ```
* Set the following vim settings
  ```shell
  vim ~/.vimrc
  set nu
  set expandtab
  set shiftwidth=2
  set tabstop=2
  ```
* Bookmark most used Kubernetes document pages
* Skip the question for later after the second troubleshooting
* Change namespace in the kubectl context as required
  ```shell
  kubectl config set-context --current --namespace {{namespace}}
  ```

## Short Names For Kubernetes Objects

| Object                   | Short Name |
|--------------------------|------------|
| Pods                     | po         |
| ReplicaSets              | rs         |
| Deployments              | deploy     |
| Services                 | svc        |
| Namespaces               | ns         |
| Network Policies         | netpol     |
| Persistent Volumes       | pv         |
| Persistent Volume Claims | pvc        |
| Service Accounts         | sa         |
