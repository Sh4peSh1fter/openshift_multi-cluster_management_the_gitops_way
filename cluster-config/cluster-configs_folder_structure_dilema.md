# the cluster-config folder structure dilema
## questions
1. what is the best folder structure for the cluster-config?

## sample background
we have 2 environments (cluster per env).
we also have 3 configs we want to apply on each environment.

## thinking process
there are 2 main ways to structure this folder:
1. env per config - folder for each configuration, and under each configuration will be layers (for each environment).
```
...
├── cluster-config
│   ├── node-config
│   │   ├── base
│   │   │   ├── kustomization.yaml (points to current files)
│   │   │   └── file1.yaml, file2.yaml, ...
│   │   └── overlays
│   │       ├── acm
│   │       │   ├── kustomization.yaml (points to cluster-config->node-configuration->base)
│   │       │   └── patched-file1.yaml
│   │       ├── test
│   │       ├── dev
│   │       ├── qa
│   │       └── prod
│   ├── storage-config
│   └── network-config
...
```
2. conf per env - folder for each environment, and under each environment will be all the configuration folders for that environment.
```
...
├── cluster-config
│   ├── base
│   │   ├── node-confg
│   │   │   ├── kustomization.yaml (points to current files)
│   │   │   └── file1.yaml, file2.yaml, ...
│   │   ├── storage-config
│   │   └── network-config
│   ├── overlays
│   │   ├── acm
│   │   │   ├── node-config
│   │   │   │   ├── kustomization.yaml (points to cluster-config->base->node-configuration)
│   │   │   │   └── patched-file1.yaml
│   │   │   ├── storage-config
│   │   │   └── network-config
│   │   ├── test
│   │   ├── dev
│   │   ├── qa
│   │   └── prod
...
```

## guiding questions:
1. in how many places a developer of specific environment needs to go to see what configuration is applied in his environment?
2. in how many places should he write a patch for a configuration?

## conclusion
