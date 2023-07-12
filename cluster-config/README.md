# cluster-config
## cluster administration
### cluster management
1. acm - subscription, namespace, operator group, multi cluster hub
### resource management
1. kubelet (?) - machine config pool, kubelet config
2. machine-autoscaler
### system configuration
1. chrony-ntp
2. infra-nodes
3. project-template
### operator hub (?)
1. operator-hub
### auth / identity management / user management
1. ldap - oauth
2. local-users
### permmision management
1. rbac - cluster admin role binding
### security
1. acs - subscription, namespace, operator group, secured cluster, stackrox central services
2. cert-manager-operator
### gitops
1. gitops-controller (gitops operator)
### monitoring / observability
1. es-operator (elastic search operator) - subscription, namespace, operator group
2. openshift-monitoring - config map
### logging
1. logging-operator - subscription, namespace, operator group, cluster logging
### DR / backup
1. etcd-backup - namespace, service account, cluster role, cluster role binding, cronjob
2. oadp-backup
### network
1. ingress - ingress controller
2. proxy - custom ca
### storage
1. odf - subscription, namespace, operator group, lvm cluster
### image registry
1. image-registry - https://www.youtube.com/watch?v=7i7RYoHZ5Ig - image registry config, pvc


## cluster observation



## cluster testing
### file integrity monitor
https://sysdig.com/blog/file-integrity-monitoring/
1. file integrity operator



# other operators that would be nice to know
1. openshift data foundation
2. rhsso
3. openshift service mesh
4. openshift compliance operator
5. Group Sync Operator - subscription, namespace, operator group, redhatcop group sync
6. custom metrics autoscaler operator (keda)



# sources
1. openshift infra gitops example by stakater
    * https://github.com/stakater/openshift-infra-gitops-example
2. openshift gitops example
    * https://github.com/hyperkineticnerd/gitops
3. redhat cop gitops catalog
    * https://github.com/redhat-cop/gitops-catalog
