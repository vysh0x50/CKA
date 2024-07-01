## Docker vs ContainerD

- Initially, Kubernetes and Docker were tightly coupled, and Kubernetes did not support any other container solutions.
- As Kubernetes gained popularity, users required other runtime engines such as rkt to work with it, so Kubernetes created the "Container Runtime Interface (CRI)".
- CRI allows any vendor to work with Kubernetes as long as it follows OCI standards.
- OCI stands for "Open Container Initiative" which contains imagespec and runtimespec.
    - imagespec - Specification on how an image should be built.
    - runtimespec - Defines standards on developing runtime.

![alt text](image.png)
- Because Docker was not designed to support CRI, they introduced a workaround known as "dockershim" to keep Docker compatible with Kubernetes.
- ContainerD is CRI compatible, so it is used as a separate engine to run containers.
- Dockershim was removed from the Kubernetes v1.24 release, removing support for Docker.
- ContainerD was originally part of Docker, but it is now a separate project within the CNCF.
- You can install containerD without installing Docker, but you will still get Docker's features.
- CLI - ctr
    - ctr comes with containerD
    - Not very user friendly
    - Only supports limited features
- ctr tool is soley made for debugging containerD. The nerdctl provides stable and human-friendly user experience.

![alt text](image-1.png)
- CLI - nertctl
    - nerdctl provides a Docker like CLI for containerD
    - nerdctl supports docker compose
    - nerdctl supports newest features in containerD
        - Encrypted container images
        - Lazy pulling
        - P2P image distribution
        - Image signing and verifying
        - Namespace in Kubernetes

![alt text](image-2.png)
- CLI - crictl
    - crictl provides CLI for CRI compatible container runtimes
    - Installed separately
    - Used to inspect and debug container runtimes and not to create containers ideally
    - Works across different runtimes

![alt text](image-3.png)
- docker cli vs crictl

![alt text](image-4.png)

![alt text](image-5.png)

- Before v1.24

![alt text](image-6.png)
- From v1.24

![alt text](image-8.png)
- Summary

![alt text](image-9.png)

## ETCD
- ETCD is a distributed reliable key-value store that is Simple, Secure and Fast
- key-value structure is different from traditional tabular/RDBMS structure
- key-value store contains individual data and updating it won't affect other ones
- Using json, yaml etc to retrieve key-value data
- To install ETCD
    - Download binaries
    - Extract
    - Run ETCD service
    - Defualt port 2379
- ETCDCTL can interact with ETCD Server using 2 API versions - Version 2 and Version 3.  By default its set to use Version 2. Each version has different sets of commands.
- To set the right version of API, set the environment variable ETCDCTL_API command export ETCDCTL_API=3
- Apart from that, you must also specify path to certificate files so that ETCDCTL can authenticate to the ETCD API Server. The certificate files are available in the etcd-master at the following path.
    - --cacert /etc/kubernetes/pki/etcd/ca.crt     
    - --cert /etc/kubernetes/pki/etcd/server.crt     
    - --key /etc/kubernetes/pki/etcd/server.key
- ./etcdctl set key1 value1 (v2)- To set a key-value - ./etcdctl put key1 value1 (v3)
- ./etcdctl get key1 - To retrieve key-value
- ./etcdctl - To view more options
- Every command you run using "kubectl get" is retrieving values from ETCD cluster

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

## Kube Controller Manager
- In Kubernetes, a Controller is a process that continuosly monitors the state of various components within the system and work towards bringing the whole system to the desired functioning state.
- For example: 
    - Node Controller:
        - A Node-Controller monitor the status of all nodes and take necessary actions to keep the application running. It does that through kube-apiserver. It checks the status of node in every 5 seconds (Node Monitor Period) and if there is no heartbeat then controller wait for 40 seconds (Node Monitor Grace Period) to mark the node as UNREACHABLE. After a node mark as UNREACHABLE then it gives 5 minutes (Pod Eviction Timeout) to come back and if it doesn't, it removes the Pods assigned to that node and provision them on the healthy node.
    - Replication Controller:
        - Responsible for monitoring the status of Replica Set and ensure that desired number of Pods are available at all time within the set.
        - If a Pod dies, it creates another one.
- There are many Controllers in Kubernetes and all are under Kube-Controller Manager.

## Kube Scheduler
- Scheduler is responsible for deciding which Pod goes to which Node. It doesn't actually place the Pod on the Node.
- Scheduler filter nodes and rank nodes.

## Kubelet
- Register the Node with the Kubernetes cluster
- When it receives instruction to load a container or Pod on the Node, it requests the Container Runtime Engine to pull the required image and run it.
- Monitor the state of the Pods and container in it and reports the kube-apiserver in a timely basis.
- Kubelet needs to install manually in worker nodes.

## Kube Proxy
- Is a process runs on each node in the cluster
- The job of kube-proxy is to look for new services and everytime a new service is created, it creates appropriate rules on each node to forward traffic from services to backend pods.
- Using iptable rules

## Pods
- Pod has a one to one relationship with containers
- To scale up, you create a new pod (You don't add additional container to scale up the application)
- To scale down, you delete existing pod
- There can be multi-container Pods, Not for scale up task but there may be situation where you want to run some helper container along with your application container. It can communicate each other in localhost network and can use same storage space.

![alt text](image-10.png)

- Example: pod-definition.yml
```
apiVersion: v1 --------> String
kind: Pod --------> String
metadata:
  name: myapp-pod --------> Dictionary
  labels:         --------> Dictionary
    app: myapp   
    type: front-end
spec:
  containers: -------> List/Array
     - name: nginx-container
      image: nginx
```
- kubectl create -f pod-definition.yml
- kubectl get pods
- kubectl describe pod myapp-pod

