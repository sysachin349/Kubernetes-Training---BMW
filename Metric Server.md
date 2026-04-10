## Metrics Server

**Metrics Server** is a **cluster-wide aggregator of resource usage data** in Kubernetes.  
It collects **CPU and Memory usage** from each node and pod and exposes them via the Kubernetes API.

> ⚠️ It is **not for long-term monitoring** — it provides **real-time, short-term metrics only**.


## Why Metrics Server is Important?

Metrics Server is a **core component** required for:

- 📈 **Horizontal Pod Autoscaler (HPA)**  
  Automatically scales pods based on CPU/Memory usage  

- ⚖️ **Resource Monitoring**  
  View live usage of nodes and pods  

- 🔍 **Basic Observability**  
  Quick insights using `kubectl top`


## How Metrics Server Works

1. **Kubelet (on each node)** exposes metrics via API  
2. **Metrics Server** collects metrics from all nodes  
3. Metrics are stored **in-memory (temporary)**  
4. Kubernetes exposes them via **Metrics API**  
5. Users/Admins access via:
   ```bash
   kubectl top node
   kubectl top pod
   ```
## Architecture Flow

Kubelet (Node Metrics)
        ↓
Metrics Server (Aggregator)
        ↓
Kubernetes API Server
        ↓
kubectl top / HPA


## Key Characteristics
⚡ Lightweight and low overhead
⏱️ Provides real-time metrics only
🧠 Designed for autoscaling decisions
🚫 Not a full monitoring solution


## What Metrics Does It Provide?

- 🖥️ Node Level Metrics
  - CPU usage
  - Memory usage
- 📦 Pod Level Metrics
  - CPU consumption per pod
  - Memory consumption per pod 


---

## Task 1: Install Metrics Server 

Apply the official Metrics Server components:
```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
Check the status of the metrics server
```
kubectl -n kube-system get po
```
Download the patch for the Mtrics Server
```
wget https://gist.githubusercontent.com/initcron/1a2bd25353e1faa22a0ad41ad1c01b62/raw/008e23f9fbf4d7e2cf79df1dd008de2f1db62a10/k8s-metrics-server.patch.yaml
```
Apply the patch
```
kubectl patch deploy metrics-server -p "$(cat k8s-metrics-server.patch.yaml)" -n kube-system
```

ReCheck if Metrics Server pods are running

```
kubectl -n kube-system get po
```
> Wait until the metrics-server pod is in Running state


Check Node Metrics

```
kubectl top node
```
>👉 Displays:
>  - CPU usage of nodes
>  - Memory usage of nodes

Check Pod Metrics

```
kubectl top pod
```
>👉 Displays:
> - CPU usage per pod
> - Memory usage per pod 

Advanced Commands: 

Sort the output on CPU Utilization
```
kubectl top pod -A --sort-by cpu
```
Sort the output on Memory Utilization
```
kubectl top pod -A --sort-by memory
```
To check the total utilization by all pods
```
kubectl top pod --sum=true -A
```
To check all the options
```
kubectl top pod --help
```
---

## Troubleshooting Tips

- ❌ kubectl top not working
👉 Check Metrics Server pod logs:
```
kubectl logs -n kube-system deploy/metrics-server
```

- ❌ TLS errors
👉 Ensure patch is applied correctly

- ❌ No metrics shown
👉 Wait 1–2 minutes after deployment


