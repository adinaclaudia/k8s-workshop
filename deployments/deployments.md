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

By default, deployments do a `RollingUpdate` when something changes in the definition. Another type of deployment strategy is `Recreate`.

```
$ kubectl rollout status deploy/nginx
$ kubectl rollout history deploy nginx
$ kubectl set image deployment nginx nginx=nginx:1.11.5 --record
$ kubectl rollout history deploy nginx
$ kubectl rollout status deploy nginx
$ kubectl rollout undo deploy nginx --to-revision=3
$ kubectl describe deploy nginx
```

#### Fine-tuning the rolling update

```
minReadySeconds: 5
strategy:
# indicate which strategy we want for rolling update
type: RollingUpdate
rollingUpdate:
    maxSurge: 1
    maxUnavailable: 1
```

* **minReadySeconds**:
    * specifies the minimum number of seconds for which a newly created Pod should be ready without any of its containers crashing, for it to be considered available
    * defaults to 0
* **maxSurge**:
    * amount of pods more than the desired number of Pods
    * this field can be an absolute number or a percentage
    * defaults to 25%
    * ex. maxSurge: 1 means that there will be at most 4 pods during the update process if replicas is 3
* **maxUnavailable**:
    * amount of pods that can be unavailable during the update process
    * defaults to 25%
    * this field can be a absolute number or a percentage
    * this field cannot be 0 if maxSurge is set to 0
    * ex. maxUnavailable: 1 means that there will be at most 1 pod unavailable during the update process


### Scaling

```
$ kubectl scale deployment nginx --replicas=6

$ kubectl autoscale deployment nginx --min=10 --max=15 --cpu-percent=80
$ kubectl get deploy nginx --watch
$ kubectl get hpa
```
[Autoscaling](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/): Horizontal Pod Autoscaler automatically scales the number of pods in a replication controller, deployment or replica set based on observed CPU utilization (or, with beta support, on some other, application-provided metrics).