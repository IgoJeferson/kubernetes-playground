# Auditing

## What's Kubernetes auditing?

- Kubernetes auditing provides a security-relevant chronological set of records documenting the sequence of activities that have affected the system 
- Can help answer what happened, why, by whom, on what, where, how it was initiated, and where was it going?
- The kube-apiserver performs auditing according to the Audit Policy and sends records to the Audit Backend

## The audit policy

- The audit policy defines what should be logged
- The audit policy is defined in a .yaml file like any other Kubernetes object but since this is auditing, kubectl won't help you much here!
    - Auditing is defined on the kube-apiserver, it's an option that's passed to kube-apiserver when it starts, this varies by cluster
    - Luckily, for minikube, we just pass a few options (like our Audit Policy) when we start minikube
- Nearly anything you can create or do can be audited in Kubernetes

## The audit backend

- Audit Backends export audit events to external storage
- Out of the box, kube-apiserver provides two backends
    - Logs - which writes events to a disk
    - Webhooks - sends events to an external API/system

## Practical Example

- Let's set up a simple Audit Policy to log all requests regarding Metadata, this will let us see what Audit data looks like without overload
- We'll configure minikbue to log it to a file on our system
- Our steps:
    1. Stop minikube (if running)
    2. Define an Audit Policy
    3. Start minikube with audit policy defined and appropriate options

### Tutorial

```shell
minikube stop
mkdir -p ~/.minikube/files/etc/ssl/certs
cat <<EOF > ~/.minikube/files/etc/ssl/certs/audit-policy.yaml
# Log all requests at the Metadata level.
apiVersion: audit.k8s.io/v1
kind: Policy
rules:
- level: Metadata
EOF
minikube start \
  --extra-config=apiserver.audit-policy-file=/etc/ssl/certs/audit-policy.yaml \
  --extra-config=apiserver.audit-log-path=-
kubectl logs kube-apiserver-minikube -n  kube-system | grep audit.k8s.io/v1
```

* Putting the file into the `~/.minikube/files/` directory triggers the [file sync mechanism](https://minikube.sigs.k8s.io/docs/tasks/sync/) to copy the `audit-policy.yaml` file
from the host onto the minikube node. When the API server container starts I'll mount the `/etc/ssl/certs` directory from the minikube node and can thus read the audit policy file. 

* You most likely want to tune the [Audit Policy](https://kubernetes.io/docs/tasks/debug-application-cluster/audit/#audit-policy) next as the one used in this tutorial is quite verbose.
To use a new file you need to stop and start minikube.

You can use jq to format the json file. Like:

``` kubectl logs kube-apiserver-minikube -n  kube-system | grep audit.k8s.io/v1 | jq .```

https://github.com/JanAhrens/minikube/commit/b0b6dd493e8ba1d8569552c65b7eb4be003ef2e6