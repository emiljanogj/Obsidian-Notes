#Kubernetes 

---
**Note**: Used so that we don't have to create a PV manually before a PVC

- **AWS-EBS**
```yaml

apiVersion: storage.k8s.io/v1  
kind: StorageClass  
metadata:  
name: standard  
provisioner: kubernetes.io/aws-ebs  
parameters:  
type: gp2  
fsType: ext4  
reclaimPolicy: Delete  
volumeBindingMode: Immediate  
allowVolumeExpansion: true  
mountOptions:  
- debug
```

- **Azure**
```yaml

apiVersion: storage.k8s.io/v1  
kind: StorageClass  
metadata:  
name: azure-standard  
provisioner: kubernetes.io/azure-disk  
parameters:  
storageaccounttype: Standard_LRS  
reclaimPolicy: Delete  
volumeBindingMode: Immediate  
allowVolumeExpansion: true
```

- **NFS**
```yaml

apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: example-nfs
provisioner: example.com/external-nfs
parameters:
  server: nfs-server.example.com
  path: /share
  readOnly: "false"
```

- **Creating PVC**
```yaml

apiVersion: v1  
kind: PersistentVolumeClaim  
metadata:  
name: my-pvc  
spec:  
accessModes:  
- ReadWriteOnce  
resources:  
requests:  
storage: 10Gi  
storageClassName: standard
```

- **Using the PVC in a Pod**
```yaml

apiVersion: v1  
kind: Pod  
metadata:  
name: my-pod  
spec:  
containers:  
- name: my-container  
image: nginx  
volumeMounts:  
- mountPath: /usr/share/nginx/html  
name: my-storage  
volumes:  
- name: my-storage  
persistentVolumeClaim:  
claimName: my-pvc
```
