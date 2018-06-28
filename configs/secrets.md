## Secrets

Objects of type secret are intended to hold sensitive information, such as passwords, OAuth tokens, and ssh keys. 


Kubernetes automatically creates secrets which contain credentials for accessing the API and it automatically modifies your pods to use this type of secret.

`$ kubectl get secrets` -> default-token of type kubernetes.io/service-account-tokens

```
$ export USER=admin; export PASSWORD=test; kubectl create secret generic user-pass --from-literal="user=${USER}" --from-literal="password=${PASSWORD}"

$ kubectl get secret user-pass -oyaml

$ echo $USER > ./configs/secrets/user.txt && echo $PASSWORD > ./configs/secrets/pass.txt && kubectl create secret generic user-pass-files --from-file=./configs/secrets/user.txt --from-file=./configs/secrets/pass.txt

$ kubectl get secret user-pass-files -oyaml
```

### Decoding a secret

```
$ kubectl get secret user-pass -ojsonpath="{.data.user}" | base64 --decode
```

### Using secrets in pods

```
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: mypod
    image: redis
    volumeMounts:
    - name: foo
      mountPath: "/etc/foo"
      readOnly: true
  volumes:
  - name: foo
    secret:
      secretName: mysecret
```

```
apiVersion: v1
kind: Pod
metadata:
  name: secret-env-pod
spec:
  containers:
  - name: mycontainer
    image: redis
    env:
      - name: SECRET_USERNAME
        valueFrom:
          secretKeyRef:
            name: mysecret
            key: username
      - name: SECRET_PASSWORD
        valueFrom:
          secretKeyRef:
            name: mysecret
            key: password
```

### Restrictions:
* Secret API objects reside in a namespace. They can only be referenced by pods in that same namespace.
* Secrets must be created before they are consumed in pods as environment variables unless they are marked as optional. References to Secrets that do not exist will prevent the pod from starting.
* Individual secrets are limited to 1MB in size.
* Secrets are available to other users using the same serviceaccount.

### Risks:

* In the API server secret data is stored as plaintext in etcd; therefore:
    * Administrators should limit access to etcd to admin users
    * Secret data in the API server is at rest on the disk that etcd uses; admins may want to wipe/shred disks used by etcd when no longer in use
* If you configure the secret through a manifest (JSON or YAML) file which has the secret data encoded as base64, sharing this file or checking it in to a source repository means the secret is compromised. Base64 encoding is not an encryption method and is considered the same as plain text.
* Applications still need to protect the value of secret after reading it from the volume, such as not accidentally logging it or transmitting it to an untrusted party.
* A user who can create a pod that uses a secret can also see the value of that secret. Even if apiserver policy does not allow that user to read the secret object, the user could run a pod which exposes the secret.
* If multiple replicas of etcd are run, then the secrets will be shared between them. By default, etcd does not secure peer-to-peer communication with SSL/TLS, though this can be configured.
* Currently, anyone with root on any node can read any secret from the apiserver, by impersonating the kubelet. It is a planned feature to only send secrets to nodes that actually require them, to restrict the impact of a root exploit on a single node.
