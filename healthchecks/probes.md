## Container probes

A Probe is a diagnostic performed periodically by the kubelet on a Container

The kubelet can optionally perform and react to two kinds of probes on running Containers:

* **livenessProbe**: Indicates whether the Container is running. If the liveness probe fails, the kubelet kills the Container, and the Container is subjected to its restart policy. If a Container does not provide a liveness probe, the default state is Success.

* **readinessProbe**: Indicates whether the Container is ready to service requests. If the readiness probe fails, the endpoints controller removes the Pod’s IP address from the endpoints of all Services that match the Pod. The default state of readiness before the initial delay is Failure. If a Container does not provide a readiness probe, the default state is Success.

Probes can be:
* *ExecAction*: Executes a specified command inside the Container. The diagnostic is considered successful if the command exits with a status code of 0.

* *TCPSocketAction*: Performs a TCP check against the Container’s IP address on a specified port. The diagnostic is considered successful if the port is open.

* *HTTPGetAction*: Performs an HTTP Get request against the Container’s IP address on a specified port and path. The diagnostic is considered successful if the response has a status code greater than or equal to 200 and less than 400.

Use-cases:
* If you’d like your Container to be killed and restarted if a probe fails, then specify a liveness probe, and specify a restartPolicy of Always or OnFailure.

* If you’d like to start sending traffic to a Pod only when a probe succeeds, specify a readiness probe. In this case, the readiness probe might be the same as the liveness probe, but the existence of the readiness probe in the spec means that the Pod will start without receiving any traffic and only start receiving traffic after the probe starts succeeding.

* If your Container needs to work on loading large data, configuration files, or migrations during startup, specify a readiness probe.

* If you want your Container to be able to take itself down for maintenance, you can specify a readiness probe that checks an endpoint specific to readiness that is different from the liveness probe.

