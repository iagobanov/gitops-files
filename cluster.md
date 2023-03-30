## Description:

In this topic, we will explore the Flux configurations within the GitHub repository. We will examine the contents and purpose of each file in the Applications and Add-ons folders, offering a clear understanding of the repository's structure and how it contributes to managing Kubernetes deployments through GitOps principles.

- **Objective**: Gain a deeper understanding of the Flux configurations in the provided GitHub repository, specifically the Applications and Add-ons folders, to improve your ability to manage containerized applications using Flux in Kubernetes clusters.

Repository Overview: https://github.com/lusoal/eks-cluster-upgrades-reference-arch

## Applications Folder

1. `01-sample-app1.yaml` - This file defines a Kubernetes deployment for a sample application called "nginx". It contains the specifications for the deployment, such as the desired number of replicas, labels, and the container image to be used. Additionally, it includes resource limits and requests, as well as the necessary environment variables.

4. `kustomization.yaml` - This file is a Kustomize configuration file that simplifies the management of Kubernetes resources. It lists the resource files to be included and any necessary patches or customizations. In this case, it references app1.yaml, app2.yaml, and ingress.yaml, ensuring they are applied together as a single configuration.

## Add-ons Folder

1. `metrics-server.yaml` - This file configures and deploys the Metrics Server add-on, a cluster-wide aggregator of resource usage data. Metrics Server is responsible for collecting and storing metrics such as CPU and memory usage from Kubernetes nodes and pods. These metrics are essential for autoscaling and monitoring the performance of your cluster. The file includes the necessary Kubernetes resources to deploy Metrics Server, such as the Deployment, Service, and ServiceAccount.

2. `kustomization.yaml` - This file is a Kustomize configuration file that lists the resources to be included when applying the Add-ons folder configuration. In this case, it references metrics-server.yaml, ensuring that the Metrics Server add-on is applied as part of the configuration.

## Clusters Folder
This folder contains the Flux configuration files specific to a Kubernetes cluster named "cluster-demo". Each cluster in your infrastructure should have its own corresponding folder within the Clusters directory.

1. **flux-system** - This subfolder contains files related to the Flux setup.
    - `gotk-components.yaml` - This file contains the necessary Kubernetes resources for installing the Flux components in "cluster-demo". It includes the CustomResourceDefinitions (CRDs), RBAC rules, and deployments for Flux components such as the source-controller, kustomize-controller, and notification-controller.

    - `gotk-sync.yaml` - This file defines the synchronization configuration for the Flux GitRepository and Kustomization resources in "cluster-demo". It specifies the target GitHub repository, the branch to sync, and the path to the Kustomize configuration for both the Applications and Add-ons folders.

2. `kustomization.yaml` - This file is a Kustomize configuration file that lists the resources to be included when applying the Flux configuration for "cluster-demo". It references the flux-system/gotk-components.yaml and flux-system/gotk-sync.yaml files.

With this understanding of the Clusters folder and flux-system subfolder, you can better manage multi-cluster environments using Flux. You can create a separate folder for each of your Kubernetes clusters and maintain cluster-specific Flux configurations in their respective directories. This approach ensures that each cluster's Flux setup is properly version-controlled and tailored to the cluster's specific requirements.
