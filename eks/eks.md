
## Creating EKS cluster

**Important**

When an Amazon EKS cluster is created, the IAM entity (user or role) that creates the cluster is added to the Kubernetes RBAC authorization table as the administrator. Initially, only that IAM user can make calls to the Kubernetes API server using kubectl. Also, the Heptio Authenticator uses the AWS SDK for Go to authenticate against your Amazon EKS cluster. If you use the console to create the cluster, you must ensure that the same IAM user credentials are in the AWS SDK credential chain when you are running kubectl commands on your cluster.

____

Configure an AWS profile called eks:
```
# region = us-east-1
# output = json

$ aws configure --profile eks
$ export AWS_PROFILE=eks
``` 

Log into AWS and create a Key (ssh-key)

To install heptio-authenticator-aws for Amazon EKS:

```
$ curl -o heptio-authenticator-aws https://amazon-eks.s3-us-west-2.amazonaws.com/1.10.3/2018-06-05/bin/darwin/amd64/heptio-authenticator-aws
$ chmod +x ./heptio-authenticator-aws
$ cp ./heptio-authenticator-aws /usr/local/bin/heptio-authenticator-aws && export PATH=/usr/local/bin:$PATH
```

Create a Role ``` eks_manage_YOUR_NAME ```

Edit ``` eks/cli-input.json ```

Than run:
```
$ aws eks create-cluster --cli-input-json "$(cat eks/cli-input.json)"

{
    "cluster": {
        "status": "CREATING", 
        "name": "eks-test-X", 
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
        "arn": "arn:aws:eks:us-east-1:276108315310:cluster/eks-test-X", 
        "createdAt": 1530250190.917
    }
}

$ aws eks describe-cluster --name eks-test-X --query cluster.status
```

Create a kubeconfig using steps from here: https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html

```
$ aws sts get-caller-identity
$ aws eks describe-cluster --name eks-test-X --query cluster.endpoint
$ aws eks describe-cluster --name eks-test-X  --query cluster.certificateAuthority.data
$ mkdir -p ~/.kube
```

Edit the kubeconfig code block below into it.

```
apiVersion: v1
clusters:
- cluster:
    server: <endpoint-url>
    certificate-authority-data: <base64-encoded-ca-cert>
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: aws
  name: aws
current-context: aws
kind: Config
preferences: {}
users:
- name: aws
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1alpha1
      command: heptio-authenticator-aws
      args:
        - "token"
        - "-i"
        - "<cluster-name>"
      env:
        - name: AWS_PROFILE
          value: "eks"
```





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


