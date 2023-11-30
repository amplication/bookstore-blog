## title: 

Extending GitOps: Effortless continuous integration and deployment on Kubernetes

## introduction:

Over the last decade, there have been notable shifts in the process of delivering source code. One of the more recent adaptations on the deployment aspect of this process has been the declarative and version controlled description of an application's desired infrastructure state and configuration - commonly referred to as 'GitOps'. This approach has gained popularity in the context of cloud-native applications and container orchestration platforms, such as Kubernetes, where managing complex, distributed systems can be challenging.

As this desired state is off declarative nature, it points to a specific/static version of that application. Which has great advantages, namely the fact that it makes it easy to roll back to a previous state, audit changes before they are made and maintain a reproducible setup. But how do we move to a newer version of an application without the need for manual version adjustments?

This is where Argo CD Image Updater comes in, it will verify if a more recent version of a container image is available, subsequently triggers the necessary updates of the applicable Kubernetes resources and optionally reflects these changes in the assosciated version control.

## overview:

Prior to diving into the technical implementation, let's establish an overview of the GitOps process and highlight the role of Argo CD Image Updater within this process.

### Default GitOps

The first part of process starts with a developer modifying the source code of the application and pushing the changes back to the version control system. Subsequently, this action initiates a workflow or pipeline that both constructs and assesses the application. The outcome is an artifact in the form of a container image, which is subsequently pushed to an image registry.

In a second - detached - part of the process, the cluster configuration repository is the single source of truth regarding the the _desired state_ of the application configuration. Argo CD will periodically monitor the Kubernetes cluster to see if the _live state_ differs from the _desired state_. When there is a difference, depending on the synchronisation strategy Argo CD will try to revert back to the _desired state_.

![gitops-default-overview](assets/gitops-default-overview.png)

### Extended GitOps

Compared to the default process, in this extended variant another Argo CD component is added to the Kubernetes cluster. The Argo CD Image Updater component will verify if a more recent version of a container image exists within the image registry. If such version is identified, the component will either directly or indirectly update the running application. In the next section we'll delve into the configuration options for the Argo CD Image Updater aswell as the implementation of the component.

![gitops-extended-overview](assets/gitops-extended-overview.png)

## implementation:

Before the technical implementation we'll familiarize ourself with the configuration options Argo CD Image Updater provides. This configuration can be found in two concepts, the `write back method` and `update strategy`. Both have options tailored to specific situation, so it is good to understand what the options are and how that equates to the technical implementation.

**Write back methods**

At the moment of writing Argo CD Image Updater supports two methods of propagating the new versions of the images to Argo CD. These methods also refered to as _write back_ methods are `argocd` & `git`. 

- `argocd`: this default _write back_ method is pseudo-persistent - when deleting an application or synchronizing the configuration in version control, any changes made to an application by Argo CD Image Updater will be gone - making it best suitable for imperatively created reasources. This default method doesn't require additional configuration.

- `git`: the other _write back_ method is the persistent/declarative option, when the a more recent version of a container image is identified, Argo CD Image Updater will store the parameter override along the application's resource manifests. It will store the override in a file named `.argocd-source-<application-name>.yaml`, reducing the risk of a merge conflict in the application's reouces manifests. To change the _write back_ method the an annotation needs to be set on the Argo CD `Application` resource. In addition the branch the to commit back to can optionally be changed from the default value `.spec.source.targetRevision` of the application.

    ```
    argocd-image-updater.argoproj.io/write-back-method: git
    ```

> [!NOTE]
> When using the `git` write back method, credentials configured for Argo CD will be re-used. A dedicated set of credentials can be provided, this and more configuration can be found in the [documentation](https://argocd-image-updater.readthedocs.io/en/stable/basics/update-methods).

**Update strategies**

In addition to the choice of which write back method to use we need to decide on a update strategy. This strategy defines how Argo CD Image Updater finds new versions of an image that is to be updated. Currently four methods are supported; `semver`, `latest`, `digest`, `name`.

- `semver`:

- `latest`:

- `digest`:

- `name`:
