## title: 

Extending GitOps: Effortless continuous integration and deployment on Kubernetes

## introduction:

Over the last decade, there have been notable shifts in the process of delivering source code. One of the more recent adaptations on the deployment aspect of this process has been the declarative and version controlled description of an application's desired infrastructure state and configuration - commonly referred to as 'GitOps'. This approach has gained popularity in the context of cloud-native applications and container orchestration platforms, such as Kubernetes, where managing complex, distributed systems can be challenging.

As this desired state is off declarative nature, it points to a specific/static version of that application. Which has great advantages, namely the fact that it makes it easy to roll back to a previous state, audit changes before they are made and maintain a reproducible setup. But how do we move to a newer version of an application without the need for manual version adjustments?

This is where Argo CD Image Updater comes in, it detects when a new container image is available, triggers the necessary updates of the applicable Kubernetes resources and optionally reflects these changes in the assosciated version control.

## overview:

Prior to diving into the technical implementation, let's establish an overview of the GitOps process and highlight the role of Argo CD Image Updater within this process.

### Default GitOps

The first part of process starts with a developer modifying the source code of the application and pushing the changes back to the version control system. Subsequently, this action initiates a workflow or pipeline that both constructs and assesses the application. The outcome is an artifact in the form of a container image, which is subsequently pushed to an image registry.

In a second - detached - part of the process, the cluster configuration repository is the single source of truth regarding the cluster 

...

![light-gitops-overview](gitops.png)
![dark-gitops-overview](image.png)

### Extended GitOps



## implementation:

At the moment of writing Argo CD Image Updater supports two methods of propagating the new versions of the images to Argo CD. These methods also refered to as write back methods are `argocd` & `git`. 

Selecting an update strategy: `semver`, `latest`, `digest`, `name`.