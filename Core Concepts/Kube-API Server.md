## Kube-API Server
- Request flow for "kubectl get nodes"
    1. The kubectl command utility communicates with the kube-api server, which authenticates and validates the request.
    2. Retrieve data from the etcd cluster and respond with the requested information.
- Request flow of creating a Pod using POST api request
    1. kube-apiserver authenticates and validates the request first.
    2. Kube-apiserver creates a pod object without assigning it to a node.
    3. Updates the information in the etcd cluster and notifies the user that the pod was created.
    4. The scheduler continuously monitors the kube-apiserver and detects that a new pod is created with no node assigned.
    5. The scheduler identifies the appropriate node to place the new pod and communicates with kube-apiserver.
    6. kube-apiserver updates information in the etcd cluster.
    7. Kube-apiserver sends the information to kubelet in the appropriate working node.
    8. Kubelet creates the pod and instructs the Container Runtime Engine to deploy the application image.
    9. Once completed, kubelet updates information back to the kube-apiserver.
    10. Kube-apiserver updates information back to the etcd cluster.
- kube-apiserver is the only component directly communicate with etcd cluster
