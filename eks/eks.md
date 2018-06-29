
## Creating EKS cluster

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