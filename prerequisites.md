## Prerequisites

For the first iteration we assume you are running a Linux distribution or MacOS and have the following tools already installed:

1. Kubectl: https://kubernetes.io/docs/tasks/tools/install-kubectl/
2. Minikube: https://kubernetes.io/docs/tasks/tools/install-minikube/
    * Install minikube: `brew cask install minikube`
    * Install driver (virtualbox did not work because of MacOS High Sierra issue https://forums.virtualbox.org/viewtopic.php?f=8&t=84092): https://github.com/kubernetes/minikube/blob/master/docs/drivers.md#hyperkit-driver
        ```
        curl -LO https://storage.googleapis.com/minikube/releases/latest/docker-machine-driver-hyperkit \
        && chmod +x docker-machine-driver-hyperkit \
        && sudo mv docker-machine-driver-hyperkit /usr/local/bin/ \
        && sudo chown root:wheel /usr/local/bin/docker-machine-driver-hyperkit \
        && sudo chmod u+s /usr/local/bin/docker-machine-driver-hyperkit
        

        minikube start --vm-driver=hyperkit
        minikube stop
        ```
    * Issues with hyperkit (and workarounds): https://github.com/kubernetes/minikube/issues/1926
3. AWS CLI: https://docs.aws.amazon.com/cli/latest/userguide/installing.html (recommend using bundled installer)