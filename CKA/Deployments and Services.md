#Kubernetes

---

- To generate a `yaml` file we use the following command:
```bash
kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml
```


### Services

- Note the difference between **NodePort**, **Port** and **TargetPort** in the image below:
![[Pasted image 20240926090457.png]]

### Difference Between NodePort, ClusterIP, Service

- **NodePort**
	- Exposes the service to external clients by opening a port in the node.
	- Access via `{node-IP:port}`
- **ClusterIP**
	- Used only for internal pods communication
	- It's not exposed to external clients
- **LoadBalancer**
	- Works only if we're using Kubernetes on a cloud platform
![[Pasted image 20240926090717.png]]
- To expose a deployment with a service using imperative command we use the following command:
```bash
kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml
```

- To create a service we use the following command:
```bash
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
```
**Note:** We then need to modify the `yaml` file to specify the selector


