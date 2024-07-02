- Replication Controller helps us to run multiple instances of a Pod thus ensuring High Availability. 
- It ensures that specified number of Pods are running in the cluster at all time.
- Either if it's a single Pod or Multiple Pods, if a pod fails then Replication Controller spin up a new Pod.
- Replication Controller helps to scale up our application by bringing up new Pods in the same node as well as in other nodes too. 
- Replication Controller and Replica Sets are created to achieve same thing. Where Replication Controller is the old technology where as Replica Sets is the new one.
- Sample Replication Controller yaml manifest
```
	 apiVersion: v1
	 kind: ReplicationController
	 metadata:
	   name: myapp-rc
	   lables:
	     app: myapp
		 type: front-end
	 spec:
	   template:
	     metadata:
		   name: myapp-pod
		   lables:
		     app: myapp
			 type: front-end
		 spec:
		   containers:
		   - name: nginx-container
			 image: nginx
	   replicas: 3
```
- Sample Replica Set yaml manifest

```
	 apiVersion: v1
	 kind: ReplicationController
	 metadata:
	   name: myapp-rc
	   lables:
	     app: myapp
		 type: front-end
	 spec:
	   template:
	     metadata:
		   name: myapp-pod
		   lables:
		     app: myapp
			 type: front-end
		 spec:
		   containers:
		   - name: nginx-container
			 image: nginx
	   replicas: 3
	   selector:
	     matchLabels:
	       type: front-end				 
```
- The main difference b/w RC and RS is in "selector" field. 
- Replica Set monitor the Pods which have the labels assigned in selector field and if any of it fails then RS bring up a new one. 
- How to scale up Replica Set
	- `> kubectl replace -f replicaset-definition.yml`
	- `> kubectl scale --replicas=6 -f replicaset-definition.yml`
	- `> kubectl scale --replicas=6 replicaset myapp-replicaset`
