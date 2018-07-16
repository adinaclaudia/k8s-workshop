## Ingress

An Ingress is a collection of rules that allow inbound connections to reach the cluster services.

Ingress can provide load balancing, SSL termination and name-based virtual hosting.

In order for the Ingress resource to work, the cluster must have an Ingress controller running: https://kubernetes.github.io/ingress-nginx/deploy/

### Add nginx ingress controller 

[Helm initialization](../helm/helm.md)

```
$ helm install stable/nginx-ingress --name my-ingress-controller -f ./eks/nginx-ingress-controller-values.yml
```

It may take a few minutes for the LoadBalancer IP to be available. When connected to a cloud provider, it will automatically create a LoadBalancer
You can watch the status by running `kubectl --namespace default get services -o wide -w my-ingress-controller-nginx-ingress-controller`


### Add ingress

```
$ kubectl apply -f ingress/ingress.yaml 

$ kubectl get ing

# Go to https://<ingressIP>/testpath in the browser
```

aws eks --profile eks describe-cluster --name eks-test

### TLS
You can secure an Ingress by specifying a secret that contains a TLS private key and certificate. Currently the Ingress only supports a single TLS port, 443, and assumes TLS termination. You can integrate with [kube-lego](https://github.com/jetstack/kube-lego) that creates and renewes TLS certificates automatically for ingress rules.

You can also manage TLS certificates manually by creating secrets that you use in the ingress.

```
  apiVersion: v1
  kind: Secret
  metadata:
    name: example-tls
    namespace: foo
  data:
    tls.crt: <base64 encoded cert>
    tls.key: <base64 encoded key>
  type: kubernetes.io/tls
```