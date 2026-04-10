#  Helm 3 & WordPress Deployment on Kubernetes 


## 📘 What is Helm?

**Helm** is the **package manager for Kubernetes**, similar to how `apt` or `yum` work for Linux.  
It allows you to define, install, and upgrade even the most complex Kubernetes applications using **Helm charts** — preconfigured YAML templates.

### ⚙️ Helm Concepts

| Term | Description |
|------|--------------|
| **Chart** | A Helm package containing Kubernetes manifests |
| **Repository** | A collection of Helm charts (like Bitnami) |
| **Release** | A running instance of a chart in a Kubernetes cluster |



---

### 🧩 Task 1: Installing Helm 3 on Kubernetes

 Update System Packages
```bash
apt update
```
 Download Helm Binary

Download the Helm binary for Linux 386 architecture:
```bash
wget https://get.helm.sh/helm-v3.11.3-linux-386.tar.gz
```
Extract the Tarball
```bash
tar -xvzf helm-v3.11.3-linux-386.tar.gz
```
Verify Extraction

Check the extracted files to ensure Helm binary exists:
```bash
ls
```
Move Helm Binary to /bin

Make Helm accessible system-wide:
```bash
mv linux-386/helm /bin/
```

Verify Installation

Confirm Helm is installed correctly:
```bash
helm version
```

✅ Output Example:

version.BuildInfo{Version:"v3.11.3", GitCommit:"...", GoVersion:"go1.20"}

### 🌐 Task 2: Install WordPress using Helm
 Verify Helm Installation
```bash
helm version
```

Add Bitnami Helm Repository

Add the official Bitnami charts repository:
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```
Verify Repositories
```bash
helm repo list
```

Install WordPress Chart

Install the WordPress application with the release name my-wordpress:

```bash
helm install my-wordpress bitnami/wordpress
```
List Helm Releases
```bash
helm list
```
🧾 Verification Steps

 Check Kubernetes Resources
```bash
kubectl get all
```
Verify Pods

Ensure all pods are running:

```bash
kubectl get pods
```

Verify Deployment

Check if the frontend WordPress pods are part of a deployment:
```bash
kubectl get deploy
```

Verify Services
```bash
kubectl get svc
```

If your cluster supports LoadBalancer, note the EXTERNAL-IP to access WordPress.

⚙️ Modify Service Type (Optional)

Change the WordPress service type from LoadBalancer to NodePort:
```bash
kubectl edit svc my-wordpress
```

Then verify the changes:
```bash
kubectl get svc
```

🌍 Access WordPress:

```bash
http://<Public-IP-of-Node>:<NodePort>
```
> e.g. http://3.85.9.5:32123

### 🧹 Cleanup

List Installed Helm Releases
```bash
helm ls
```
Uninstall WordPress

Remove the release:
```bash
helm uninstall my-wordpress
```
Confirm Deletion
```bash
helm ls
```
Remove Repository
```bash
helm repo remove bitnami
```

🔹 Step 5: Remove Helm (Optional)
```bash
sudo apt-get remove helm
```
 
