## Prerequisites

For the first iteration we assume you are running a Linux distribution or MacOS and have the following tools already installed:

0. Docker must be installed.
1. Kubectl (>= 1.10): https://kubernetes.io/docs/tasks/tools/install-kubectl/

For windows: 
`curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.11.0/bin/windows/amd64/kubectl.exe` and add it to path

2. Minikube (0.28.0): https://kubernetes.io/docs/tasks/tools/install-minikube/
    
    For MacOS:
    * Install minikube: `brew cask install minikube`
    * Install driver (virtualbox did not work because of MacOS High Sierra issue https://forums.virtualbox.org/viewtopic.php?f=8&t=84092): https://github.com/kubernetes/minikube/blob/master/docs/drivers.md#hyperkit-driver
        ```
        curl -LO https://storage.googleapis.com/minikube/releases/latest/docker-machine-driver-hyperkit \
        && chmod +x docker-machine-driver-hyperkit \
        && sudo mv docker-machine-driver-hyperkit /usr/local/bin/ \
        && sudo chown root:wheel /usr/local/bin/docker-machine-driver-hyperkit \
        && sudo chmod u+s /usr/local/bin/docker-machine-driver-hyperkit
        

        minikube start --vm-driver=hyperkit
        minikube addons list
        minikube addons enable heapster
        minikube addons enable dashboard
        minikube addons enable ingress
        ```
    * Issues with hyperkit (and workarounds): https://github.com/kubernetes/minikube/issues/1926
3. AWS CLI (>1.15.32): https://docs.aws.amazon.com/cli/latest/userguide/installing.html (recommend using bundled installer)
4. Helm 2.9.1 : https://docs.helm.sh/using_helm/#installing-helm
