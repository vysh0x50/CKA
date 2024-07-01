## Kubelet
- Register the Node with the Kubernetes cluster
- When it receives instruction to load a container or Pod on the Node, it requests the Container Runtime Engine to pull the required image and run it.
- Monitor the state of the Pods and container in it and reports the kube-apiserver in a timely basis.
- Kubelet needs to install manually in worker nodes.
