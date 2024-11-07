![[Pasted image 20240928122808.png]]

**Note: Mapping Docker -> K8s**
- `ENTRYPOINT` -> `command`
- `CMD` -> `args`


### ENV Variables

![[Pasted image 20240928122950.png]]

- Example of importing a key from a config map:
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: "2024-09-28T10:42:08Z"
  labels:
    name: webapp-color
  name: webapp-color
  namespace: default
  resourceVersion: "892"
  uid: 34132d0f-5b47-4ba3-8468-3881e6bc7835
spec:
  containers:
  - env:
    - name: APP_COLOR
      valueFrom:      
        configMapKeyRef:
          name: webapp-config-map
          key: APP_COLOR
```

- To import all the keys we use the `envFrom` field as follows:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: dapi-test-pod
spec:
  containers:
    - name: test-container
      image: registry.k8s.io/busybox
      command: [ "/bin/sh", "-c", "env" ]
      envFrom:
      - configMapRef:
          name: special-config
  restartPolicy: Never
```
