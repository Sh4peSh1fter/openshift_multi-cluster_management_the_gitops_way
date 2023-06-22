# ocp management
ocp management using the gitops method. we will manage our openshift clusters with our configuration manifests stored nicely in git, and applying them using the openshift gitops operator (argocd).
in this repo we want to define our cluster management, governance, policy management, cluster configuration and applications deployment.

## keywords
keywords can help people find this repo, describeit from different angles, and can be used to search and find related sources we can learn from.

openshift gitops, ocp gitops, openshift one touch provisioning, ocp gitops

## how does it work?
the logical steps look like this:
1. create gitops operator
2. apply its configuration on itself
3. create application sets for the infra-core configuration manifests
4. apply infra-core configuration manifests on the clusters

to do so, we:
1. "kubectl apply" the bootstrap default overlay, which applys the openshift gitops operator.
2. then it applys the gitops operator components, such as application sets (in argocd).
3. the application sets applys the infra-core configuration manifests on the clusters.

## use cases
if you already have a gitops operator / argocd running and managing everything, maybe you need only the "infra-core".

## getting started
so overall, we only need to manualy prompt the first apply - applying the gitops operator which manages it all.
run this bash script that applys with kustomize the gitops operator. 
> it loops through the "kubectl apply" command with sleep because if our cluster doesn't have some of the manifests for the gitops operator while we applying it, it throws errors until they are created in this process.
```
until kubectl apply -k https://<git-server-url>/<path-to-bootstrap-overlay>; do sleep 3; done
```
replace the \<git-server-url> with the url of the server where the repo is stored, and \<path-to-bootstrap-overlay> with to full path to your bootstrap overlay that you apply from.
for example, in my case it will look like this:
```
until kubectl apply -k https://github.com/sean_sosis/ocp-management/bootstrap/overlays/acm; do sleep 3; done
```

check the results by running in your cluster:
```
kubectl get applications -n openshift-gitops
```
you should get #?

take a look at the argocd ui under its route:
```
oc get route openshift-gitops-server -n openshift-gitops --template='https://{{.spec.host}}'
```

## folder structure
```
ocp-management/
├── bootstrap
│   ├── base
│   │   ├── kustomization.yaml (points to current files)
│   │   └── # gitops operator subscription, argo instances, gitops operator rbac policy
│   └── overlays
│       └── acm
│           ├── kustomization.yaml (points to bootstrap->base, components->application-sets, and components->argocd-projects)
│           └── # patched stuff as in the base
├── components
│   ├── application-sets
│   │   ├── kustomization.yaml (points to current files)
│   │   └── # application sets 
│   └── argocd-projects
│       ├── kustomization.yaml (points to current files)
│       └── # argocd projects
├── infra-core
│   ├── gitops-controller
│   │   └── kustomization.yaml (points to bootstrap->overlays->acm)
│   └── # other infra-core configuration manifests for the cluster, such as "resource reservation", "storage management", "proxy configuration", and so on
└── app-workloads
    └── # application workloads that will run on the cluster
```

### bootstrap
the "bootstrap" folder contains all the minimal manifests needed for the gitops operator.
the idea is that it will be one point of start, after which the openshift gitops operator will deploy all the other manifests of the infra-core configuration.

* base - where the common yamls stored.
* overlays - configuration for specific clusters.
> note: why is there a seperation to base and overlays? not sure for now.

### components
the "components" folder contains additional components of the gitops operator - argocd specific configuration, such as appliocation sets, argocd projects, etc.

* application-sets - yamls that generate application set from each manifest folder under the infra-core or the app-workloads folders.
* argocd-projects - argocd project that will be created in the argocd.
> note: why is this folder a standalone from the "bootstrap" folder? why all the stuff related to the gitops operator are not under one folder? not sure for now.


### infra-core
the "infra-core" folder contains the cluster configuration manifests, that will be applied on our clusters.

* gitops-controller - contains the kustomization file that points into the overlay with which we bootstraped our gitops operator. thus the argocd manages itself.


### app-workloads
the "app-workloads" folder contains the application manifests, that will run on our clusters.


# sources
1. openShift gitops repo example by christian hernandez 
    * https://github.com/christianh814/example-openshift-go-repo
2. gitops standards
    * https://github.com/gnunn-gitops/standards
3. ocp gitops demo
    * https://github.com/froberge/ocp-gitops-demo
4. openshift gitops example
    * https://github.com/jstakun/openshift-gitops
5. openshift gitops usage guide
    * https://github.com/redhat-developer/gitops-operator/blob/master/docs/OpenShift%20GitOps%20Usage%20Guide.md
6. Cloud Native Toolkit - GitOps Production Deployment Guide
    * https://github.com/cloud-native-toolkit/multi-tenancy-gitops
7. one-touch-provisioning
    * https://devops.talksplus.com/storage/files/presentations/1670420587.pdf
    * https://github.com/one-touch-provisioning/otp-gitops
    * https://github.com/rampadc/otp-gitops
8. GitOps for organizations: provisioning and configuring OpenShift clusters automatically
    * https://cloud.redhat.com/blog/gitops-for-organizations-provisioning-and-configuring-openshift-clusters-automatically
9. Configuring Openshift cluster with ApplicationSets using Helm+Kustomize and ACM Policies
    * https://cloud.redhat.com/blog/configuring-openshift-cluster-with-applicationsets-using-helmkustomize-and-acm-policies
10. gitops-fleet-samples
    * https://github.com/jnpacker/gitops-fleet-samples


# todo
1. maybe change the term "application set" into "aas" for "argo application set". because the application set's yaml file's name is kinda long.
2. same idea, "argo project" -> "ap".
3. "infra-core" -> "cluster-config" or "cluster-conf".
4. to get around the errors thrown at the "apply bootstrap" for the first time running, doesn't argocd's sync waves method solve them? giving a correct order of sync.
5. find a way to minimize the "dilemas" throughout this readme. tried to use dropdowns but it doesn't digest markdown inside of it. maybe point to seperate readme for each.
6. maybe there is no need for the "use cases" section.
7.we can give a cluster as a service with acm resources (cluster claim, cluster deployment and install config secrets (and maybe more stuff))