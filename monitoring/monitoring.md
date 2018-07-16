## Monitoring

Application monitoring can be done through:

* The **resource metrics pipeline** that provides a limited, short-term, in-memory store for metrics collected via the `metrics-server`. This can discover all nodes and query the kubelet for CPU and memory usage.
* A **full monitoring pipeline**, such as Prometheus, which gives access to richer metrics. Prometheus can natively monitor kubernetes, nodes, and prometheus itself. It provides a robust query language and a built-in dashboard for querying and visualizing your data. Prometheus is also a supported data source for Grafana.

[Prometheus's](https://prometheus.io/docs/introduction/overview/) main **features** are:

* a multi-dimensional data model with time series data identified by metric name and key/value pairs
* a flexible query language to leverage this dimensionality
* no reliance on distributed storage; single server nodes are autonomous
* time series collection happens via a pull model over HTTP
* pushing time series is supported via an intermediary gateway
* targets are discovered via service discovery or static configuration
* multiple modes of graphing and dashboarding support

The Prometheus ecosystem consists of multiple **components**, many of which are optional:

* the main Prometheus server which scrapes and stores time series data
* client libraries for instrumenting application code
* a push gateway for supporting short-lived jobs
* special-purpose exporters for services like HAProxy, StatsD, Graphite, etc.
* an alertmanager to handle alerts
various support tools

![Prometheus Architecture](../images/prometheus.png "Prometheus Architecture")

[Short intro on Helm](../helm/helm.md)

[Prometheus helm chart](https://github.com/kubernetes/charts/tree/master/stable/prometheus)

[Grafana helm chart](https://github.com/kubernetes/charts/tree/master/stable/grafana)

```
$ helm repo update

$ helm install stable/prometheus --name my-prometheus --namespace=monitoring --set nodeExporter.enabled=false

$ kubectl apply -nmonitoring -f monitoring/grafana-datasource-configmap.yaml
$ kubectl apply -nmonitoring -f monitoring/grafana-dashboard-configmap.yaml

$ helm install stable/grafana --name my-grafana -f ./monitoring/grafana-values.yml --namespace monitoring

# get admin password
$ kubectl get secret -nmonitoring my-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

$ kubectl get svc -nmonitoring my-grafana
$ minikube service list # get IP and port of Grafana and open in browser
```