### Pods

Pods are the smallest deployable units of computing that can be created and managed in Kubernetes.

Pods consist of one or more containers which share IP address, access to storage and namespace. Typically, one container inside a pod runs an application, while others support the primary application. 
* Containers inside a pod can communicate via localhost.
* Containers start in parallel, no way to determine order of startup.
* Pods can specify init containers, that run before the normal containers.

In general, users shouldn’t need to create pods directly, because they aren't durable entities by themselves. Controllers, such as deployments, provide self-healing and replication.

### Pod phases:
* **Pending**	The Pod has been accepted by the Kubernetes system, but one or more of the Container images has not been created. This includes time before being scheduled as well as time spent downloading images over the network, which could take a while.
* **Running**	The Pod has been bound to a node, and all of the Containers have been created. At least one Container is still running, or is in the process of starting or restarting.
* **Succeeded**	All Containers in the Pod have terminated in success, and will not be restarted.
* **Failed**	All Containers in the Pod have terminated, and at least one Container has terminated in failure. That is, the Container either exited with non-zero status or was terminated by the system.
* **Unknown**	For some reason the state of the Pod could not be obtained, typically due to an error in communicating with the host of the Pod.

### Pod conditions:
* **PodScheduled**: the Pod has been scheduled to a node;
* **Ready**: the Pod is able to serve requests and should be added to the load balancing pools of all matching Services;
* **Initialized**: all init containers have started successfully;
* **Unschedulable**: the scheduler cannot schedule the Pod right now, for example due to lacking of resources or other constraints;
* **ContainersReady**: all containers in the Pod are ready.

### Pod lifetime: 
* In general pods run until someone terminates them.
* Exceptions are Jobs, where pods are one-time running and are expected to terminate. Job pods can be restarted automatically if they terminate with failure, until they succeed or until a timeout occurs.


```
$ kubectl apply -f deployments/nginx.yaml
$ kubectl get po nginx --watch 
$ kubectl get po nginx -oyaml
$ kubectl describe po nginx
$ kubectl logs -c docker -f nginx
$ kubectl logs -c nginx -f nginx
$ kubectl port-forward nginx 3000:80
$ curl http://localhost:3000
$ kubectl exec -it nginx -- bash
$ kubectl delete po nginx
$ kubectl get events
```

### Restart policy
Applies to all containers in a pod:
* Always: default
* OnFailure
* Never

### Init containers

A Pod can have multiple Containers running apps within it, but it can also have one or more Init Containers, which are run before the app Containers are started.
Init Containers are exactly like regular Containers, except:

* They always run to completion.
* Each one must complete successfully before the next one is started

```
$ kubectl apply -f deployments/nginx-with-init.yaml
$ kubectl get po nginx-init --watch
$ kubectl describe po nginx-init
$ kubectl logs -c docker nginx-init | less
$ kubectl delete po nginx-init
```

### Image private registries
Create a secret that contains the credentials to pull images from the registry.
```
$ kubectl create secret docker-registry myregistrykey --docker-server=DOCKER_REGISTRY_SERVER --docker-username=DOCKER_USER --docker-password=DOCKER_PASSWORD --docker-email=DOCKER_EMAIL
secret "myregistrykey" created.
```
Use the secret in the pod template:
```
apiVersion: v1
kind: Pod
metadata:
  name: foo
  namespace: awesomeapps
spec:
  containers:
    - name: foo
      image: janedoe/awesomeapp:v1
  imagePullSecrets:
    - name: myregistrykey
```

### Compute resources
When you specify a Pod, you can optionally specify how much CPU and memory (RAM) each Container needs. When Containers have resource requests specified, the scheduler can make better decisions about which nodes to place Pods on. And when Containers have their limits specified, contention for resources on a node can be handled in a specified manner.

Limits and requests for CPU resources are measured in cpu units (fractions are allowed). One cpu, in Kubernetes, is equivalent to:

* 1 AWS vCPU
* 1 GCP Core
* 1 Azure vCore
* 1 IBM vCPU
* 1 Hyperthread on a bare-metal Intel processor with Hyperthreading

Limits and requests for memory are measured in bytes. You can express memory as a plain integer or as a fixed-point integer using one of these suffixes: E, P, T, G, M, K. You can also use the power-of-two equivalents: Ei, Pi, Ti, Gi, Mi, Ki. For example, the following represent roughly the same value:

`128974848, 129e6, 129M, 123Mi`

#### Scheduling pods on nodes:

`sum of resource requests for containers in a pod < node maximum capacity for each resource`

When you create a Pod, the Kubernetes scheduler selects a node for the Pod to run on. Each node has a maximum capacity for each of the resource types: the amount of CPU and memory it can provide for Pods. The scheduler ensures that, for each resource type, the sum of the resource requests of the scheduled Containers is less than the capacity of the node.

#### Enforcing resource limits

* If a Container exceeds its memory limit, it might be terminated. If it is restartable, the kubelet will restart it, as with any other type of runtime failure.

* If a Container exceeds its memory request, it is likely that its Pod will be evicted whenever the node runs out of memory.

* A Container might or might not be allowed to exceed its CPU limit for extended periods of time. However, it will not be killed for excessive CPU usage.

[Go to deployments](./deployments.md)