#Kubernetes 

---

- Defining a **Role**
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  creationTimestamp: "2024-09-28T20:53:48Z"
  name: kube-proxy
  namespace: kube-system
  resourceVersion: "260"
  uid: 8984eefb-42ae-4f3b-a853-2c3acbfeeb45
rules:
- apiGroups:
  - ""
  resourceNames:
  - kube-proxy
  resources:
  - configmaps
  verbs:
  - get
```

- Defining a **Rolebinding** to attach the above role to a group
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  creationTimestamp: "2024-09-28T20:53:48Z"
  name: kube-proxy
  namespace: kube-system
  resourceVersion: "261"
  uid: dfef3754-e319-492e-a50a-94a3f4bf446e
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: kube-proxy
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: Group
  name: system:bootstrappers:kubeadm:default-node-token
```


- To check if a given user(i.e `dev-user`) can do a certain action(i.e. `list pods`) we use the following command:
```bash
controlplane ~ ➜  kubectl auth can-i list pods --as dev-user
no
```
