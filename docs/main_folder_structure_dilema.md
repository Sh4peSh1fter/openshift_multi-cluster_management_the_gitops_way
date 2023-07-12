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
├── cluster-config
│   ├── gitops-controller
│   │   └── kustomization.yaml (points to bootstrap->overlays->acm)
│   └── # other cluster-config configuration manifests for the cluster, such as "resource reservation", "storage management", "proxy configuration", and so on
└── admin-apps
    └── # application workloads that will run on the cluster
```


or


1. bootstrap
2. infra-config
3. clusters
4. services
5. policies
6. applications