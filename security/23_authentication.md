# Authentication

## Supported Authentication Methods

* Static password file
* Static toke file
* Certificates
* 3rd party identity services

## Static Password File

* A CSV files with user details
* Cluster should be configured to use the static password file
* CSV File contains:
  * password
  * username
  * user id
  * group name (Optional)

e.g.,
```text
password1,username1,userId1,group1
password2,username2,userId2,group2
password3,username3,userId3,group1
password4,username4,userId4,group2
```

## Authenticate to Kube API Server With Username & Password

```shell
curl -v -k https://master-node-ip:6443/api/v1/pods -u username:password
```

## Static Token File

* A CSV file with token information
* Cluster should be configured to use the static token file
* CSV file contains:
  * token
  * username
  * user id
  * group name (Optional)

e.g.,
```text
token1,username1,userId1,group1
token2,username2,userId2,group2
token3,username3,userId3,group1
token4,username4,userId4,group2
```

## Authenticate to Kube API Server With Token

```shell

curl -v -k https://master-node-ip:6443/api/v1/pods -H "Authorization: Bearer <token>"
```