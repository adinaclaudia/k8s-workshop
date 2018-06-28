# Kubernetes workshop

This is a first version of a Kubernetes workshop. Feedback is welcome!

[Prerequisites](./prerequisites.md)

[Kubectl cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

Agenda:
* [Overview](./overview.md)
* [Installation](./installation.md)
* [Context and configuration](./cluster/context.md)
* [Cluster information](./cluster/information.md)
* [Pods, replica sets and deployments](./deployments/pods.md)
* [Configmaps, secrets](./configs/configmap.md)
* [Labels and selectors](./labels/labels.md)
* [Volumes](./volumes/volumes.md)
* [Namespaces](./namespaces/namespaces.md)
* Services, service discovery
* [Healthchecks](./healthchecks/probes.md)
* Ingress controller, load balancer
* Pet sets, Stateful sets, daemon sets
* Monitoring
* Debugging

Future:
* [Node selection](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/)
* [Taints and tolerations](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)
* RBAC
* [Zero downtime deployments with Jenkins](https://kubernetes.io/blog/2018/04/30/zero-downtime-deployment-kubernetes-jenkins/)

Other resources:
* https://github.com/aws-samples/aws-workshop-for-kubernetes/blob/master/developer-path.adoc