## Namespaces

Namespaces isolate groups and the resources they have access to work with. The only resources that are not in a namespaces are nodes and persistent volumes (and namespaces themselves).

* *default* namespace is where all resources are assumed, unless specified otherwise
* *kube-public* namespace is readable by all, even if not authenticated
* *kube-system* namespace contains infrastructure pods

```
$ kubectl get ns
$ kubectl create ns test
$ kubectl apply -ntest -f ./deployments/deployment.yaml
$ kubectl get pods -n test
$ kubectl get pods --all-namespaces
$ kubectl delete deploy nginx -ntest
$ kubectl delete ns test
```


```
$ kubectl config set-context <context-name> --namespace=test
```