```bash
vi multi-ctr-pod.yml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    env: qa
  name: multi-ctr-pod
spec:
  containers:
  - image: nginx
    name: con1
    ports:
    - containerPort: 80
  - name: con2
    image: busybox
    command: 
    - "sh"
    - "-c"
    - "sleep 5000"
    # command: ["sh", "-c", "sleep 5000"]
```

```bash
kubectl apply -f multi-ctr-pod.yml
```

---
