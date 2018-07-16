
## Creating EKS cluster

**Important**

When an Amazon EKS cluster is created, the IAM entity (user or role) that creates the cluster is added to the Kubernetes RBAC authorization table as the administrator. Initially, only that IAM user can make calls to the Kubernetes API server using kubectl. Also, the Heptio Authenticator uses the AWS SDK for Go to authenticate against your Amazon EKS cluster. If you use the console to create the cluster, you must ensure that the same IAM user credentials are in the AWS SDK credential chain when you are running kubectl commands on your cluster.

____

Add to `~/.aws/config`
```
[profile eks]
region = us-east-1
output = json
``` 

Duplicate the credentials in `~/.aws/credentials` and rename profile to `eks`

Run:
```
$ export AWS_PROFILE=eks
$ aws eks create-cluster --cli-input-json "$(cat eks/cli-input.json)"

{
    "cluster": {
        "status": "CREATING", 
        "name": "eks-test", 
        "certificateAuthority": {}, 
        "roleArn": "arn:aws:iam::276108315310:role/eks_manage", 
        "resourcesVpcConfig": {
            "subnetIds": [
                "subnet-4062ce6c", 
                "subnet-5866c302", 
                "subnet-0e009b46"
            ], 
            "vpcId": "vpc-c03593b9", 
            "securityGroupIds": [
                "sg-0356bb72"
            ]
        }, 
        "version": "1.10", 
        "arn": "arn:aws:eks:us-east-1:276108315310:cluster/eks-test", 
        "createdAt": 1530250190.917
    }
}


$ aws eks describe-cluster --name eks-test --query cluster.status
```

Create a kubeconfig using steps from here: https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html


### Launch worker nodes

https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html#eks-launch-workers

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: aws-auth
  namespace: kube-system
data:
  mapRoles: |
    - rolearn: <ARN of instance role (not instance profile)>
      username: system:node:{{EC2PrivateDNSName}}
      groups:
        - system:bootstrappers
        - system:nodes
```


```
$ kubectl apply -f aws-auth-cm.yaml
$ kubectl get nodes --watch
```

