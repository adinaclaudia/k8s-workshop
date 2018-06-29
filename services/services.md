## Services

Service is an abstraction that enables connectivity between different layers of your application that is deployed in different pods, e.g. frontend pods that need to communicate with backend pods.

The service defines a logical set of pods by using a label selector and provides access to them.

### Service types
* **ClusterIP**: Exposes the service on a cluster-internal IP. Choosing this value makes the service only reachable from within the cluster. This is the default ServiceType.
* **NodePort**: Exposes the service on each Node’s IP at a static port (the NodePort, range: 30000-32767). A ClusterIP service, to which the NodePort service will route, is automatically created. You’ll be able to contact the NodePort service, from outside the cluster, by requesting `<NodeIP>:<NodePort>`.
* **LoadBalancer**: Exposes the service externally using a cloud provider’s load balancer. NodePort and ClusterIP services, to which the external load balancer will route, are automatically created. The actual creation of the load balancer happens asynchronously, and information about the provisioned balancer will be published in the Service’s .status.loadBalancer field.
* **ExternalName**: Maps the service to the contents of the externalName field (e.g. foo.bar.example.com), by returning a CNAME record with its value. No proxying of any kind is set up. This requires version 1.7 or higher of kube-dns


```
$ kubectl apply -f ./services/nginx.yaml
$ minikube ip -> get IP of minikube
# open browser at http://<minikube-IP>:32000
```

### Service discovery
* via environment variables
    * When a Pod is run on a Node, the kubelet adds a set of environment variables for each active Service; e.g.
        ```
        REDIS_MASTER_SERVICE_HOST=10.0.0.11
        REDIS_MASTER_SERVICE_PORT=6379
        REDIS_MASTER_PORT=tcp://10.0.0.11:6379
        REDIS_MASTER_PORT_6379_TCP=tcp://10.0.0.11:6379
        REDIS_MASTER_PORT_6379_TCP_PROTO=tcp
        REDIS_MASTER_PORT_6379_TCP_PORT=6379
        REDIS_MASTER_PORT_6379_TCP_ADDR=10.0.0.11
        ``` 
    * If the service is not created before the pod, the env variables will not be populated.
* via DNS, if it's installed as an add-on
    * The DNS server watches the Kubernetes API for new Services and creates a set of DNS records for each.
    * For example, if you have a Service called "my-service" in Kubernetes Namespace "my-ns" a DNS record for "my-service.my-ns" is created. 
    * FQDN available as well: `my-svc.my-ns.svc.cluster.local`


Examples that should be run on cloud cluster (don't forget to switch kube context):
```
$ kubectl apply -f ./volumes
$ kubectl apply -f ./healthchecks/node.yaml
$ kubectl apply -f ./services
```