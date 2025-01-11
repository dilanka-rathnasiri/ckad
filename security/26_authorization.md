# Authorization

* The api server evaluate all api requests against all policies

## Authorization Modes

* AlwaysAllow
* AlwaysDeny
* Node
* ABAC (Attribute-Based Access Control)
* RBAC (Role-Based Access Control)
* Webhook

## What happens when there are multiple mechanisms
* Access is denied by default
* When multiple authorization modes have been configured:
  * Check for each mode sequentially
  * If any mode explicitly approves the request => Allow and no checking with other modes
  * If any mode explicitly denies the request => Deny and no checking with other modes
  * If all the modes have no opinion on request => Deny
  * Reject response status will be HTTP 403–forbidden

## Node Authorization

* **Node authorizer** handles node authorization
* Use for kubelet api requests
* Node authorizer allows a kubelet for following API operations
  * Read operations of resources that pods are bounded to the Kubelet's node: Services, Endpoints, Nodes, Pods, and etc.
  * Write operations: Node status, Pod status, Events, and etc.

## ABAC: Attribute-Based Access Control

* Need to set attributes for each user
* Need a policy file:
  * One json object per line
  * Each line is a policy object
  * All the users refer to this policy file

## Webhook

* Authorization through 3rd party services
* e.g, open policy agent
* There is no Kubernetes native built-in mechanism
