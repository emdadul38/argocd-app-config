## ArgoCD
ArgoCD is a declarative, GitOps continuous delivery tool for Kubernetes. It helps manage and deploy applications to Kubernetes clusters by syncing the desired state defined in Git repositories with the actual state in the cluster.

#### Commands

```bash
# install ArgoCD in k8s
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# access ArgoCD UI
kubectl get svc -n argocd
kubectl -n argocd port-forward svc/argocd-server 8080:443 

# login with admin user and below token (as in documentation):
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode && echo

# you can change and delete init password

```
</br>


## ArgoCD CLI

### Installation and Setup
```bash
# Install ArgoCD CLI
brew install argocd
```

### Authentication
```bash
# Login to ArgoCD server
argocd login <ARGOCD_SERVER> --username <USERNAME> --password <PASSWORD>

# Update password
argocd account update-password
```

### Application Management
```bash
# Create a new application
argocd app create <APP_NAME> \
  --repo <GIT_REPO_URL> \
  --path <PATH_IN_REPO> \
  --dest-server <K8S_CLUSTER_URL> \
  --dest-namespace <NAMESPACE>

# Sync an application (deploy changes)
argocd app sync <APP_NAME>

# Get application status
argocd app get <APP_NAME>

# List all applications
argocd app list

# Delete an application
argocd app delete <APP_NAME>
```

### Cluster Management
```bash
# Add a new cluster
argocd cluster add <CONTEXT_NAME>

# List clusters
argocd cluster list
```

### Project Management
```bash
# Create a new project
argocd proj create <PROJECT_NAME>

# List projects
argocd proj list
```

### Other Useful Commands
```bash
# Get ArgoCD version
argocd version

# Logout from ArgoCD
argocd logout <ARGOCD_SERVER>
```

### Example Workflow
1. **Login**:
   ```bash
   argocd login localhost:8080 --username admin --password <INITIAL_PASSWORD>
   ```
2. **Create an Application**:
   ```bash
   argocd app create my-app \
     --repo https://github.com/myorg/my-repo.git \
     --path k8s/manifests \
     --dest-server https://kubernetes.default.svc \
     --dest-namespace default
   ```
3. **Sync the Application**:
   ```bash
   argocd app sync my-app
   ```

