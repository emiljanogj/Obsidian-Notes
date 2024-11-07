#Kubernetes 

---

- **ClusterRole Example**:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: storage-admin
rules:
- apiGroups:
  - '*'
  resources:
  - 'persistentvolumes'
  verbs:
  - 'list'
- apiGroups:
  - 'storage.k8s.io'
  resources:
  - 'storageclasses'
  verbs:
  - 'list'
```

**ClusterRoleBinding Example**
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: michelle-storage-admin
subjects:
- kind: User
  name: michelle
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: storage-admin
  apiGroup: rbac.authorization.k8s.io
```


- To check the **api-resources** class for a given resource we use the following command:
```bash
kubectl api-resources | grep {resource_name}
```
