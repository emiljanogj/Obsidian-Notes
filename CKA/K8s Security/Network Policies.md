#Kubernetes 

---

- Example of an **Ingress Network Policy**
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  annotations:
  name: payroll-policy
  namespace: default
spec:
  ingress:
  - from:
    - podSelector:
        matchLabels:
          name: internal
    ports:
    - port: 8080
      protocol: TCP
  podSelector:
    matchLabels:
      name: payroll
  policyTypes:
  - Ingress
```

**Note**: We have a **podSelector** outside ingress, which specified the pod where this policy is applied and a **podSelector** inside the ingress field to specify from which pod we'll allow the traffic

- Example of **Egress Network Policy**
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  annotations:
  name: internal-policy
  namespace: default
spec:
  egress:
  - to:
    - podSelector:
        matchLabels:
          name: mysql
    ports:
    - protocol: TCP
      port: 3306

  - to:
    - podSelector:
        matchLabels:
          name: payroll
    ports:
    - protocol: TCP
      port: 8080

  podSelector:
    matchLabels:
      name: internal
  policyTypes:
  - Egress
```

