## Pods
- Pod has a one to one relationship with containers
- To scale up, you create a new pod (You don't add additional container to scale up the application)
- To scale down, you delete existing pod
- There can be multi-container Pods, Not for scale up task but there may be situation where you want to run some helper container along with your application container. It can communicate each other in localhost network and can use same storage space.

<img src=../images/11.png width="700"/>

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
