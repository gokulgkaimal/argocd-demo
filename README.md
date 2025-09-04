Argo CD + Minikube (Hello App Project)


 What We Did


Started Minikube → local Kubernetes cluster.

Installed Argo CD into Minikube (argocd namespace).

Accessed Argo CD UI using port-forward.

Created a GitHub repo (argocd-demo) and added YAML files (deployment.yaml, service.yaml) directly in GitHub.

Connected Argo CD to the GitHub repo → created an Application (hello-app).

Synced and deployed app → accessed NGINX via Minikube service.

Tested GitOps → changed replicas in GitHub, Argo CD auto-synced, and pods updated.


 How We Did It


Used Minikube as Kubernetes environment.

Installed Argo CD with official manifests.

Logged into Argo CD UI using admin credentials.

Used a GitHub repo as single source of truth for Kubernetes manifests.

Argo CD tracked the repo and deployed app automatically.

Verified the app with kubectl and minikube service.

Proved GitOps by editing the GitHub YAML → Argo CD synced the cluster.

🔹 Commands Used
1. Start Minikube
minikube start --cpus=2 --memory=4096

2. Install Argo CD
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl get pods -n argocd

3. Access Argo CD UI
kubectl -n argocd port-forward svc/argocd-server 8080:443


Open: https://localhost:8080

Get initial password:

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo


Login → user: admin, pass: above.

4. GitHub Repo

Manually created argocd-demo repo on GitHub.

Added deployment.yaml and service.yaml files via GitHub UI.

5. Create Argo CD App

UI → New App:

Name: hello-app

Project: default

Repo URL: GitHub repo URL

Revision: main

Path: .

Destination: https://kubernetes.default.svc, namespace: default

6. Verify App
kubectl get pods
minikube service hello-service

7. Prove GitOps

In GitHub repo → edited deployment.yaml → changed replicas: 1 → 2.

Argo CD showed OutOfSync → Synced.

Verified:

kubectl get pods -l app=hello


✅ End Result: Argo CD deployed NGINX from GitHub into Minikube. Any change in GitHub automatically updated the cluster.
