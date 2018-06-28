## Volumes

Volumes are used for persisting data from Pods and for sharing data between pods. 

Sometimes, it is useful to share one volume for multiple uses in a single Pod. The `volumeMounts.subPath` property can be used to specify a sub-path inside the referenced volume instead of its root.

Types of volumes:
* awsElasticBlockStore
* azureDisk
* azureFile
* configMap
* emptyDir
* gitRepo
* hostPath
* nfs
* persistentVolumeClaim
* secret
* and others

### AWSElasticBlockStorage

There are some restrictions when using an awsElasticBlockStore volume:

* the nodes on which Pods are running must be AWS EC2 instances
* those instances need to be in the same region and availability-zone as the EBS volume
* EBS only supports a single EC2 instance mounting a volume

Creating an EBS volume:
```
$ aws ec2 describe-availability-zones
$ aws ec2 create-volume --availability-zone=eu-central-1a --size=10 --volume-type=gp2

{
    "AvailabilityZone": "eu-central-1a", 
    "Tags": [], 
    "Encrypted": false, 
    "VolumeType": "gp2", 
    "VolumeId": "vol-0e2ddf01833f1aa63", 
    "State": "creating", 
    "Iops": 100, 
    "SnapshotId": "", 
    "CreateTime": "2018-06-28T23:01:06.000Z", 
    "Size": 10
}
```

Using it:

```
apiVersion: v1
kind: Pod
metadata:
  name: test-ebs
spec:
  containers:
  - image: busybox
    name: test-container
    volumeMounts:
    - mountPath: /test-ebs
      name: test-volume
  volumes:
  - name: test-volume
    # This AWS EBS volume must already exist.
    awsElasticBlockStore:
      volumeID: vol-0e2ddf01833f1aa63
      fsType: ext4
```

### NFS
An nfs volume allows an existing NFS (Network File System) share to be mounted into your Pod

### persistentVolumeClaim
A persistentVolumeClaim volume is used to mount a PersistentVolume into a Pod. PersistentVolumes are a way for users to “claim” durable storage (such as a GCE PersistentDisk or an iSCSI volume) without knowing the details of the particular cloud environment.

[Go to persistent volumes](./persistentvolumes.md)