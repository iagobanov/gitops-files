## GitOps Reconciliation flow

In this step-by-step guide, we will walk through the process of GitOps reconciliation using Flux in a Kubernetes cluster. We'll modify the 01-sample-app.yaml file from the GitHub repository (https://github.com/lusoal/eks-cluster-upgrades-reference-arch) and observe Flux detecting and applying the changes.

## Flux Reconciliation

Flux reconciliation is the process by which Flux continuously monitors a Git repository for changes to Kubernetes manifests and automatically applies them to a connected cluster. When a change is detected in the repository, Flux compares the updated manifest with the current state of the cluster. If there's a discrepancy, Flux updates the cluster to match the desired state defined in the Git repository. This GitOps-based approach ensures consistency, version control, and automation in managing Kubernetes deployments, streamlining CI/CD pipelines, and reducing human error.

<p align="center">
<img src="./gitops-toolkit.png">
</p>

## Requirements

- Have Flux up and running in your cluster
- Have Flux connected to the GitHub repository containing the application manifests

## Flux changes

1. Open the 01-sample-app.yaml file in your favorite text editor. The file contains the deployment specifications, including the container image, desired number of replicas, labels, resource limits and requests, and required environment variables.

Modify the 01-sample-app.yaml file by making a change to the deployment configuration. For example, update the container name:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx-new # value updated
  name: nginx-new # value updated
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  strategy: {}
  template:
    metadata:
      labels:
        app: nginx-new # value updated
    spec:
      containers:
      - image: nginx:latest
        name: nginx
        ports:
        - containerPort: 80
        resources: {}
status: {}
```

2. Save the changes made to the 01-sample-app.yaml file.

3. Commit and push the changes to the repository:

```bash
git add 01-sample-app.yaml
git commit -m "Update app1 container image version"
git push origin main
```

4. Flux will now detect the changes made to the 01-sample-app.yaml file and start the reconciliation process. It does this by periodically polling the GitHub repository for changes. You can monitor the Flux logs to observe the reconciliation process:

```bash
kubectl -n flux-system logs -f source-controller-<ID> --since=1m
```

Replace <ID> with the actual ID of your source-controller pod.

You should see logs indicating that the new changes have been detected and applied to the cluster:

```json
{"level":"info","ts":...,"logger":"controller.gitrepository","msg":"Reconciliation finished in ...","reconciler group":"source.toolkit.fluxcd.io","reconciler kind":"GitRepository","name":"flux-system","namespace":"flux-system","commit":"<COMMIT_HASH>","revision":"main/<COMMIT_HASH>"}
```

5. Verify that the updated manifest has been applied to the cluster using kubectl:

```bash
kubectl get deployments app1 -o jsonpath='{.spec.template.spec.containers[0].image}'
```

The output should display the updated container image version.
