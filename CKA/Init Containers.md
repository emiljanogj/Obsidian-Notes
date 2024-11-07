- An **init container** is a container that is deployed/run before the main container and is destroyed after finishing its task
- It's usually used to run a specific task that is required to complete before the main container is deployed
- An example of using an init container is given below:
```yaml
apiVersion: v1
kind: Pod
metadata:
	name: myapp-pod
	labels:
		app: myapp
spec:
	containers:
	- name: myapp-container
	  image: busybox:1.28
	  command: ['sh', '-c', 'echo The app is running! && sleep 3600']
	initContainers:
	- name: init-myservice
	  image: busybox:1.28
	  command: ['sh', '-c', 'until nslookup myservice; do echo waiting for myservice; sleep 2; done;']
	- name: init-mydb
	  image: busybox:1.28
	  command: ['sh', '-c', 'until nslookup mydb; do echo waiting for mydb; sleep 2; done;']
```
