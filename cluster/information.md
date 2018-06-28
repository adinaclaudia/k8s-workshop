## Cluster information

Getting cluster information and interacting with nodes:

```
# Mark my-node as unschedulable
$ kubectl cordon my-node

# Drain my-node in preparation for maintenance
$ kubectl drain my-node

# Mark my-node as schedulable
$ kubectl uncordon my-node

# Show metrics for a given node
$ kubectl top node my-node

# Display addresses of the master and services
$ kubectl cluster-info

# Dump current cluster state to /path/to/cluster-state
$ kubectl cluster-info dump --output-directory=/path/to/cluster-state
```

## Web UI

If not already deployed, you can deploy `kubernetes-dashboard` to have a web UI for the cluster: https://github.com/kubernetes/dashboard

After it's deployed run
```
$ kubectl proxy
```
or if port 8001 is already in use
```
$ kubectl proxy -p 8002
```
then go to

http://localhost:8002/api/v1/namespaces/kube-system/services/http:kubernetes-dashboard:/proxy