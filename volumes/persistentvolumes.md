## Persistent Volumes

A *PersistentVolume (PV)* is a piece of storage in the cluster that has been provisioned by an administrator. It is a resource in the cluster just like a node is a cluster resource. PVs are volume plugins like Volumes, but have a lifecycle independent of any individual pod that uses the PV. This API object captures the details of the implementation of the storage, be that NFS, iSCSI, or a cloud-provider-specific storage system.

A *PersistentVolumeClaim (PVC)* is a request for storage by a user. It is similar to a pod. Pods consume node resources and PVCs consume PV resources. Pods can request specific levels of resources (CPU and Memory). Claims can request specific size and access modes (e.g., can be mounted once read/write or many times read-only).

### Lifecycle

* Provisioning:
    * static
    * dynamic, based on *StorageClasses* (previously configured by administrators)
* Binding: PVC binds 1-1 to a PV
* Using: pods use claims as volumes
* Reclaiming: when a PVC is deleted, the PV can be reclaimed

```
$ kubectl apply -f ./volumes/persistentvolume.yaml 
$ kubectl get pv

$ kubectl apply -f ./volumes/persistentvolumeclaim.yaml 

# check that PVC is bound to PV
$ kubectl get pvc
$ kubectl get pv

# use it in a deployment
$ kubectl apply -f ./volumes/mongo.yaml 
```

*Note:* You cannot use this PVC on minikube, as it supports only PVs of type HostPath. To try these out, you need a real cluster.

