## Installation

You can use any of Chef, Puppet, Ansible, Terraform and scripts for multiple providers are available.

The hard way:
* **Kubeadm**: https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/
* **Kops**: create k8s cluster on AWS via a single command https://github.com/kubernetes/kops
* **Kubespray** : advanced Ansible playbook to install k8s on varios OS
* **Kube-aws**: makes use of AWS Cloud Formation
* **Kubicorn**: makes use of kubeadm

The managed way:
* EKS
    * https://aws.amazon.com/blogs/aws/amazon-eks-now-generally-available/
    * https://aws.amazon.com/getting-started/projects/deploy-kubernetes-app-amazon-eks/
