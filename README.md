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

In a second - detached - part of the process, the cluster configuration repository is the single source of truth regarding the the _desired state_ of the application configuration. Argo CD will periodically monitor the Kubernetes cluster to see if the _live state_ differs from the _desired state_. When there is a difference, depending on the synchronisation strategy Argo CD will try to revert back to the _desired state_.

![gitops-default-overview](assets/gitops-default-overview.png)

### Extended GitOps





![gitops-extended-overview](assets/gitops-extended-overview.png)

## implementation:



At the moment of writing Argo CD Image Updater supports two methods of propagating the new versions of the images to Argo CD. These methods also refered to as write back methods are `argocd` & `git`.

- write back method - `argocd`:

- write back method - `git`:

In addition to the choice of which write back method to use we need to decide on a update strategy. This strategy defines how Argo CD Image Updater finds new versions of an image that is to be updated. Currently four methods are supported; `semver`, `latest`, `digest`, `name`.

- update strategy - `semver`:

- update strategy - `latest`:

- update strategy - `digest`:

- update strategy - `name`:
