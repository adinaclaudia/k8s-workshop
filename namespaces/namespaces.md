## Namespaces

Namespaces isolate groups and the resources they have access to work with.

* *default* namespace is where all resources are assumed, unless specified otherwise
* *kube-public* namespace is readable by all, even if not authenticated
* *kube-system* namespace contains infrastructure pods

```
$ kubectl get ns
$ kubectl create ns test
$ kubectl get pods --all-namespaces

```