https://argo-cd.readthedocs.io/en/stable/getting_started/

Getting started with **ArgoCD** involves setting up the tool, connecting it to a Git repository, and deploying applications to Kubernetes. Below is a step-by-step guide to help you get started:

---

### **1. Prerequisites**
1. **Kubernetes Cluster**: Ensure you have a running Kubernetes cluster (local or cloud-based).
   - Tools like Minikube, Kind, or a managed Kubernetes service (e.g., AKS, EKS, GKE) can be used.
2. **kubectl**: Install and configure `kubectl` to interact with your cluster.
3. **Git Repository**: Prepare a Git repository containing your application manifests or Helm charts.
4. **Helm (Optional)**: If using Helm charts for deployment.

---

### **2. Install ArgoCD**

#### a) Install ArgoCD in Your Kubernetes Cluster
Run the following commands to install ArgoCD using the official manifest:

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

#### b) Verify Installation
Check the status of the ArgoCD components:
```bash
kubectl get pods -n argocd
```

You should see several pods running (e.g., `argocd-server`, `argocd-repo-server`, `argocd-controller`, etc.).

---

### **3. Access the ArgoCD UI**

#### a) Expose ArgoCD Server
You can expose the ArgoCD server for access:
1. Using a `kubectl port-forward` command:
   ```bash
   kubectl port-forward svc/argocd-server -n argocd 8080:443
   ```
   Access the UI at [https://localhost:8080](https://localhost:8080).

2. Alternatively, configure an Ingress or LoadBalancer service.

#### b) Login to ArgoCD
- **Username**: `admin`
- **Password**: The initial password is the name of the `argocd-server` pod. Retrieve it with:
  ```bash
  kubectl get pods -n argocd -o name | grep argocd-server
  kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
  ```
- Change the password after logging in.

---

### **4. Connect ArgoCD to Your Git Repository**

#### a) Add a Git Repository
1. Log in to the ArgoCD UI or use the CLI.
2. Add your Git repository:
   ```bash
   argocd repo add <REPO_URL> --username <GIT_USERNAME> --password <GIT_PASSWORD>
   ```
   Replace `<REPO_URL>`, `<GIT_USERNAME>`, and `<GIT_PASSWORD>` with your repository details.

---

