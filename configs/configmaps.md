## Configmaps

ConfigMaps allow you to decouple configuration artifacts from image content to keep containerized applications portable.

You can create configmaps from:
* directories
    ```
    $ kubectl create configmap some-config --from-file=configs/some-config
    $ kubectl get cm some-config -oyaml
    $ kubectl describe cm some-config
    ```
* files
    ```
    $ kubectl create configmap some-config2 --from-file=configs/some-config/foo

    $ kubectl create configmap some-config3 --from-file=configs/some-config/foo --from-file=configs/some-config/index.html

    $ kubectl create configmap some-config4 --from-file=kubernetes.io=configs/some-config/index.html
    ```
* literal values
    ```
    kubectl create configmap env-config --from-literal=literal1=env1 --from-literal=literal2=env2
    ```


```
$ kubectl apply -f configs/nginx-with-cm-volume.yaml 

$ kubectl port-forward nginx-cm-volume 3000:80 # visit localhost:3000

$ kubectl exec -it -c nginx nginx-cm-volume -- bash
root@nginx-cm-volume:/# cd /usr/share/nginx/html
root@nginx-cm-volume:/# ls -al
root@nginx-cm-volume:/# exit

$ kubectl logs -c docker -f nginx-cm-volume | less
```

[More examples](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)

Restrictions:
* You must create a ConfigMap before referencing it in a Pod specification (unless you mark the ConfigMap as “optional”). If you reference a ConfigMap that doesn’t exist, the Pod won’t start. Likewise, references to keys that don’t exist in the ConfigMap will prevent the pod from starting.
* ConfigMaps reside in a specific namespace. A ConfigMap can only be referenced by pods residing in the same namespace.

[Go to secrets](./secrets.md)