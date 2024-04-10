# Starter Files for Project 2: Multi-Cluster Deployment using Flux v2

These are starter files for the "Project 2: Multi-Cluster Deployment using Flux v2". Since we are going to setup a new Git repository for this project, you should use these files and instructions in this README before continuing with Project 2.

Here's how the directory structure looks like:
```
.
├── clusters
│   └── development
│       ├── flux-system
│       │   ├── gotk-components.yaml
│       │   ├── gotk-sync.yaml
│       │   └── kustomization.yaml
│       └── podinfo
│           ├── deployment.yaml
│           ├── namespace.yaml
│           └── service.yaml
└── README.md
```

# Overview

Before we start with the Project 2, we have to setup our base infrastructure. If you have already completed "Project 1: Single Microservice Deployment with Flux v2", then you are already familiar with the setup.

In brief, we will set up following:
1. Setup `kind` based Kuberentes cluster on your local system
2. Bootstrap (i.e. deploy and configure Flux) Kubernetes Cluster using Flux to make it GitOps ready
3. Deploy `podinfo` application

# Instructions
1. Install `kubectl`, `kind` and `flux` command line utilities.
    Installation Guides:
    - `kubectl`: https://kubernetes.io/docs/tasks/tools/
    - `kind`: https://kind.sigs.k8s.io/docs/user/quick-start/#installation
    - `flux`: https://fluxcd.io/flux/cmd/
2. Create Kubernetes cluster using `kind` and name it `development`.
   ```
   kind create cluster --name development
   ```
3. Make sure you're working on the recently created cluster. The kubectl context name for it will be `kind-development`.
   ```
   kubectl config use-context kind-development
   ```
   You can verify the current context with the following command and check the `CURRENT` column.
   ```
   kubectl config get-contexts
   ```
4. The GitOps step! Make the cluster GitOps ready by installing and configuring Flux by using `flux bootstrap` command.

   Replace the value of the `--owner` (e.g. `manning-lp`) and `--repository` arguments appropriately.
   ```
   flux bootstrap github \
      --owner=<github-username/org-name> \
      --repository=<name-of-this-repository> \
      --path=clusters/development \
      --private
   ```

With that step, you will have:
- installed Flux in the cluster
- configured the Git repository to monitor for changes in the path `clusters/development`
- deployed the `podinfo` application which was present under `clusters/development/podinfo`

5. Check with `kubectl` that the pods are up and running. Here's the sample output from `flux-system` namespace where Flux deploys its controllers. 
    ```
    $ kubectl get pods -n flux-system
    NAME                                       READY   STATUS    RESTARTS   AGE
    helm-controller-5d8d5fc6fd-f75xw           1/1     Running   0          48m
    kustomize-controller-7b7b47f459-zf4pt      1/1     Running   0          48m
    notification-controller-5bb6647999-s776k   1/1     Running   0          48m
    source-controller-7667765cd7-5vvc5         1/1     Running   0          48m
    ```

    And, lastly, the `podinfo` application deployed in `dev` namespace.
    ```
    $ kubectl get pods -n dev        
    NAME                       READY   STATUS    RESTARTS   AGE
    podinfo-6ccb6c5f58-qgnf2   1/1     Running   0          46m
    ```



**Now, you are ready! Continue with the Project 2 milestones.**
