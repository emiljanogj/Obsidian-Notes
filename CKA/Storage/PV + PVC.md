- To create a **Physical Volume** we use the following `yaml` file:
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-log
spec:
  capacity:
    storage: 100Mi
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /pv/log
```

- **PV Access Modes**:
- ReadWriteOnce(RWO): Allows for read-write from one node(or as many podes from the same node) at a time
- ReadOnlyMany(ROX): Allows for read-only from many pods simultaneously
- ReadWriteMany(RWX): Allows for read-write from many pods simultaneously
- ReadWriteOncePod(RWOP): Allows for read-write from one specific pod only

- To create a **Physical Volume Claim** we use the following `yaml` file:
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: claim-log-1
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 50Mi
```


- To use the **PVC** in a **Pod** we use the following `yaml` file:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webapp
spec:
  containers:
    - name: event-simulator
      image: kodekloud/event-simulator
      volumeMounts:
      - mountPath: /log
        name: claim-log
  volumes:
    - name: claim-log
      persistentVolumeClaim:
        claimName: claim-log-1
```

- When deleting a **PVC**, several things may happen to its underlying **PV**:
	- **Retain** - PV is preserved but marked as "released" (data remains intact).
	- **Delete** - Both PVC and PV (along with the underlying storage) are deleted.
	- **Recycle** - PV data is wiped and made available for new claims (deprecated).


### Storage Class

- To create a **Storage Class** we use the following `yaml`:
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: delayed-volume-sc
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
```



### Note: Difference Between a PV and Volume

![[Pasted image 20241001083112.png]]

**Main Difference**: Volumes are used for shared storage across containers in a pod(i.e. not persistent across pod restarts), while PV are used for persistent volumes for pods(i.e. persistent across pod restarts)
