## Ingress

An Ingress is a collection of rules that allow inbound connections to reach the cluster services.

Ingress can provide load balancing, SSL termination and name-based virtual hosting.

In order for the Ingress resource to work, the cluster must have an Ingress controller running: https://kubernetes.github.io/ingress-nginx/deploy/


```
$ kubectl apply -f ingress/ingress.yaml 

$ kubectl get ing

# Go to https://<ingressIP>/testpath in the browser
```

### TLS
You can secure an Ingress by specifying a secret that contains a TLS private key and certificate. Currently the Ingress only supports a single TLS port, 443, and assumes TLS termination. You can integrate with [kube-lego](https://github.com/jetstack/kube-lego) that creates and renewes TLS certificates automatically for ingress rules.