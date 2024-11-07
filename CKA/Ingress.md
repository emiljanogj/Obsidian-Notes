#Kubernetes 

---

- The **rewrite-target** option is used because we want to match:

`http://<ingress-service>:<ingress-port>/watch` --> `http://<watch-service>:<port>/`

`http://<ingress-service>:<ingress-port>/wear` --> `http://<wear-service>:<port>/`

but without **rewrite-target** we would get the following:

`http://<ingress-service>:<ingress-port>/watch` --> `http://<watch-service>:<port>/watch`

`http://<ingress-service>:<ingress-port>/wear` --> `http://<wear-service>:<port>/wear`

which is not correct since `/wear` and `/watch` are not defined in `wear-service` and `watch-service`


- The `yaml` file is defined as follows:
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
	name: test-ingress
	namespace: critical-space
	annotations:
		nginx.ingress.kubernetes.io/rewrite-target: /
spec:
	rules:
	- http:
		paths:
		- path: /pay
		  backend:
			serviceName: pay-service
			servicePort: 8282
```

