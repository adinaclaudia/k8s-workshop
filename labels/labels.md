## Labels and selectors

Labels are key/value pairs that are attached to objects, such as pods. They can be used to organize and to select subsets of objects.

Labels enable users to map their own organizational structures onto system objects in a loosely coupled fashion, without requiring clients to store these mappings.

```
template:
  metadata:
    labels:
      key1: value1
      key2: value2
```

Via a **label selector**, the client/user can identify a set of objects. The label selector is the core grouping primitive in Kubernetes.

* Equality- or inequality-based requirements allow filtering by label keys and values. Matching objects must satisfy all of the specified label constraints. Operators: =,==,!=
    * environment = production
    * tier != frontend
* Set-based label requirements allow filtering keys according to a set of values. Operators: in, notin, exists
    * environment in (production, qa)
    * tier notin (frontend, backend)
    * partition
    * !partition

```
$ kubectl get po -l '!app'
$ kubectl get po -l app=nginx
```

[Services](./services/services.md) allow only equality-based selectors:
```
selector:
  app: nginx
```

Jobs, deployments, replica sets and daemon sets support set-based selectors as well:
```
selector:
  matchLabels:
    component: redis
  matchExpressions:
    - {key: tier, operator: In, values: [cache]}
    - {key: environment, operator: NotIn, values: [dev]}
```