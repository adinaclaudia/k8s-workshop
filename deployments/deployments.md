## Replica set

**ReplicaSet** is the next-generation Replication Controller. The only difference between a ReplicaSet and a Replication Controller right now is the selector support. Although still supported, Replication controllers are not recommended anymore.

A *ReplicaSet* ensures that a specified number of pod replicas are running at any given time. However, a **Deployment** is a higher-level concept that manages ReplicaSets and provides declarative updates to pods along with a lot of other useful features. Therefore, using Deployments instead of directly using ReplicaSets is recommended, unless you require custom update orchestration or don’t require updates at all. 

```
$ kubectl apply -f deployments/replicaset.yaml
$ kubectl get rs nginx -oyaml
$ kubectl describe rs nginx
$ kubectl get events
$ kubectl get po -lapp=nginx-label
$ kubectl delete rs nginx
```

## Deployments

A Deployment controller provides declarative updates for Pods and ReplicaSets. You describe a desired state in a Deployment object, and the Deployment controller changes the actual state to the desired state.

```
$ kubectl apply -f deployments/deployment.yaml
$ kubectl get deploy nginx -oyaml
$ kubectl describe deploy nginx
$ kubectl get events
$ kubectl get po -lapp=nginx
$ kubectl get po -lapp=nginx --watch
$ kubectl get deploy nginx --watch


# Change nginx image tag to 1.8.0, apply and observe events
# Change nginx image tag to 1.13, apply and observe events
```

### Rolling updates (and rollback)