# Exam Tips

## Details

- Time: 2 hours
- Questions: 19
- So, time management is critical

## Tips

- Try to do the questions with the highest marks first
- Try to do the easiest questions first
- No need of answering questions sequentially
- Note down the difficult questions with their marks in the given notepad
- Refer Kubernetes Documentation
- Read the instructions on the paper for coping from the Kubernetes Documentation
- Use **imperative commands** as much as possible because there's no time to edit YAML files
  - Don't use imperative commands for complex configurations
  - It makes things much harder
- Always write text as it is in the question
  - If the question has single quotes => write single quotes
  - If the question has double quotes => write double quotes
  - If the question has no quotes => don't use quotes
- Use a separate yaml file for each resource
  - Don't write multiple resource at same file
  - It makes troubleshooting harder
- Understand:
  - linux file systems
  - ssh
  - volume mounts
  - Use `kubectl run`
- Use the following bash aliases

  ```shell
  export ns=default
  alias k="kubectl"
  alias kn='kubectl -n $ns'
  export dr="--dry-run=client -o yaml"
  ```

- Set the following vim settings

  ```shell
  vim ~/.vimrc
  set nu
  set paste
  set hlsearch
  set ignorecase
  set expandtab
  set sw=2
  set ts=2
  ```

- Bookmark most used Kubernetes document pages
- Skip the question for later after the second troubleshooting
- Change namespace in the kubectl context as required

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
| Resource Quota           | quota      |
| Endpoints                | ep         |

## WGET

- Print result on terminal without downloading

  ```shell
  wget -O- {{url}}
  ```

- Call url in web spider mode
  - Not download the page
  - Just check it's in there

    ```shell
    wget --spider {{url}}
    ```
