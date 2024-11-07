#Kubernetes 

---

- To place a taint on a node we use the following command:
```bash
kubectl taint nodes {node-name} {key}={value}:{taint-effect}
```
The `taint-effect` can be one of the following:
- **NoSchedule** - pods that cannot tolerate this taint will not be scheduled in this node
- **PreferNoSchedule** - prefers the pods that can't tolerate the taint to not be schedule but it's not guaranteed
- **NoExecute** - the existing pods in the node that don't tolerate the taint will be re-scheduled

- To add a toleration to a pod, we add the following field under spec:
```yaml

tolerations:
- key: "app"
  operator: "Equal"
  value: "blue"
  effect: "NoSchedule"
```

- To remove a taint from a node we use the following command:
```bash
kubectl taint nodes node1 key1=value1:NoSchedule-
```


### NodeAffinity

- To set node affinity we use the following syntax in `yaml` file:
```yml
affinity:
 nodeAffinity:
  requiredDuringSchedulingIgnoredDuringExecution:
   nodeSelectorTerms:
   - matchExpressions:
     - key: size
       operator: Exists
```
