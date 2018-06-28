# Pods

Pods are the smallest deployable units of computing that can be created and managed in Kubernetes.

Pods consist of one or more containers which share IP address, access to storage and namespace. Typically, one container inside a pod runs an application, while others support the primary application. 
* Containers inside a pod can communicate via localhost.
* Containers start in parallel, no way to determine order of startup.
* Pods can specify init containers, that run before the normal containers.

Pod phases:
* **Pending**	The Pod has been accepted by the Kubernetes system, but one or more of the Container images has not been created. This includes time before being scheduled as well as time spent downloading images over the network, which could take a while.
* **Running**	The Pod has been bound to a node, and all of the Containers have been created. At least one Container is still running, or is in the process of starting or restarting.
* **Succeeded**	All Containers in the Pod have terminated in success, and will not be restarted.
* **Failed**	All Containers in the Pod have terminated, and at least one Container has terminated in failure. That is, the Container either exited with non-zero status or was terminated by the system.
* **Unknown**	For some reason the state of the Pod could not be obtained, typically due to an error in communicating with the host of the Pod.

Pod conditions:
* **PodScheduled**: the Pod has been scheduled to a node;
* **Ready**: the Pod is able to serve requests and should be added to the load balancing pools of all matching Services;
* **Initialized**: all init containers have started successfully;
* **Unschedulable**: the scheduler cannot schedule the Pod right now, for example due to lacking of resources or other constraints;
* **ContainersReady**: all containers in the Pod are ready.

Pod lifetime: 
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


[Go to deployments](./deployments.md)