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