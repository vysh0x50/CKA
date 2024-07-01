## Kube Proxy
- Is a process runs on each node in the cluster
- The job of kube-proxy is to look for new services and everytime a new service is created, it creates appropriate rules on each node to forward traffic from services to backend pods.
- Using iptable rules
