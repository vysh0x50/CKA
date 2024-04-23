In Kubernetes, the master node and worker nodes are two key components of the cluster architecture:

<img src=../images/kubernetes-cluster-architecture.svg alt="kubernetes-cluster-architecture" width="700"/>

### Master Node:
 - The master node is responsible for managing the Kubernetes cluster.
 - It oversees the scheduling and orchestration of applications across the worker nodes.
 - Key components running on the master node include the Kubernetes API server, scheduler, controller manager, and etcd.
 - The Kubernetes API server is the front end for the Kubernetes control plane. It's responsible for handling API requests, validating them, and updating the state of the API objects in etcd.
 - Etcd is a distributed key-value store used to store the cluster's configuration data and state.
 - The scheduler is responsible for placing applications onto available nodes based on resource requirements and constraints.
 - The controller manager is responsible for managing various controllers that regulate the state of the cluster, ensuring that the desired state matches the actual state.

### Worker Nodes:
 - Worker nodes are the machines (physical or virtual) in the Kubernetes cluster where application workloads are run.
 - Each worker node runs a kubelet, which is an agent responsible for managing the node and communicating with the master node.
 - The kubelet ensures that containers are running in a pod as expected. It communicates with the container runtime (e.g., Docker, containerd) to start, stop, and manage containers.
 - Worker nodes also typically run a container runtime (such as Docker or containerd) responsible for running containers.
 - Additionally, worker nodes may run other Kubernetes components like kube-proxy, which is responsible for managing network connectivity to the pods.

In summary, the master node controls and manages the cluster, while the worker nodes host the applications and execute the tasks assigned by the master node.