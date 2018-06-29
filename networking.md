Kubernetes approaches networking somewhat differently than Docker does by default. There are 4 distinct networking problems to solve:

* Highly-coupled container-to-container communications: this is solved by pods and localhost communications.
* Pod-to-Pod communications: via CNI.
* Pod-to-Service communications: this is covered by services.
* External-to-Service communications: this is covered by services.


Kubernetes is using **Container Networking Interface (CNI)** to provide container networking. CNI plugins can be used to configure the network of a pod and provide a single IP per pod. However, for pod-to-pod commiuncation, a software overlay is used to route all IPs (nodes and pods) without NAT. Networking add-ons examples:
* Weave
* Flannel
* Calico
* Romana

More documentation: https://kubernetes.io/docs/concepts/cluster-administration/networking/

