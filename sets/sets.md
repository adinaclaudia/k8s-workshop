## Stateful sets

*Previously called PetSets.*

Manages the deployment and scaling of a set of Pods, and provides guarantees about the ordering and uniqueness of these Pods.

Unlike a Deployment, a StatefulSet maintains a sticky identity for each of their Pods, that is comprised of an ordinal, a stable network identity, and stable storage. These pods are created from the same spec, but are not interchangeable: each has a persistent identifier that it maintains across any rescheduling.

```
apiVersion: apps/v1
kind: StatefulSet
```

StatefulSets are valuable for applications that require one or more of the following.

* Stable, unique network identifiers.
* Stable, persistent storage.
* Ordered, graceful deployment and scaling.
* Ordered, graceful deletion and termination.
* Ordered, automated rolling updates.

#### Ordinal Index
For a StatefulSet with N replicas, each Pod in the StatefulSet will be assigned an integer ordinal, from 0 up through N-1, that is unique over the Set.

#### Stable Network ID
If the service DNS is `myService.myNamespace.svc.cluster.local`, the pod DNS will be `myPod-{0..N-1}.myService.myNamespace.svc.cluster.local` and the pod hostname `myPod-{0..N-1}`

#### Deployment and Scaling Guarantees
* For a StatefulSet with N replicas, when Pods are being deployed, they are created sequentially, in order from {0..N-1}.
* When Pods are being deleted, they are terminated in reverse order, from {N-1..0}.
* Before a scaling operation is applied to a Pod, all of its predecessors must be Running and Ready.
* Before a Pod is terminated, all of its successors must be completely shutdown.


## Daemon set
A DaemonSet ensures that all (or some) Nodes run a copy of a Pod. As nodes are added to the cluster, Pods are added to them. As nodes are removed from the cluster, those Pods are garbage collected. Deleting a DaemonSet will clean up the Pods it created.

Some typical uses of a DaemonSet are:

* running a cluster storage daemon, such as glusterd, ceph, on each node.
* running a logs collection daemon on every node, such as fluentd or logstash.
* running a node monitoring daemon on every node, such as Prometheus Node Exporter, collectd, Datadog agent, New Relic agent, or Ganglia gmond.

#### Running Pods on Only Some Nodes
If you specify a `.spec.template.spec.nodeSelector`, then the DaemonSet controller will create Pods on nodes which match that node selector. Likewise if you specify a `.spec.template.spec.affinity`, then DaemonSet controller will create Pods on nodes which match that node affinity. If you do not specify either, then the DaemonSet controller will create Pods on all nodes.


```
$ kubectl apply -f ./sets/node-exporter.yml 
```
