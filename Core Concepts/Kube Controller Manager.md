## Kube Controller Manager
- In Kubernetes, a Controller is a process that continuosly monitors the state of various components within the system and work towards bringing the whole system to the desired functioning state.
- For example: 
    - Node Controller:
        - A Node-Controller monitor the status of all nodes and take necessary actions to keep the application running. It does that through kube-apiserver. It checks the status of node in every 5 seconds (Node Monitor Period) and if there is no heartbeat then controller wait for 40 seconds (Node Monitor Grace Period) to mark the node as UNREACHABLE. After a node mark as UNREACHABLE then it gives 5 minutes (Pod Eviction Timeout) to come back and if it doesn't, it removes the Pods assigned to that node and provision them on the healthy node.
    - Replication Controller:
        - Responsible for monitoring the status of Replica Set and ensure that desired number of Pods are available at all time within the set.
        - If a Pod dies, it creates another one.
- There are many Controllers in Kubernetes and all are under Kube-Controller Manager.
