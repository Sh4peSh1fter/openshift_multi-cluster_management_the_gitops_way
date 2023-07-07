# argocd-bootstrap chart

* argocd-application-controller
    - k8s controller that continuously monitors running applications and compared the current live state against the desired target state.
* argocd-repo-server
    - internal service which maintains a local cache of the git repository holding the application manifests, and is responsible for generating and returning the k8s manifests.
* argocd-server
    - gRPC/REST server which exposes the api consumed by the web UI, CLI and CI/CD systems.


# usage
1. helm template bootstrap/base/ > bootstrap/overlays/hub-cluster/resources.yaml
2. until kubectl apply -k bootstrap/overlays/hub-cluster/; do sleep 3; done

if you want to delete
1. kubectl delete -k bootstrap/overlays/hub-cluster/


# issues
1. while deploying the openshift gitops subscription, if the argocd instance is given a name that is defferent from the default name ("openshift-gitops"), then in addition to the argocd instance, will be deployed another instance with the default name. using the "disable_default_argocd_instance" doesnt seem to work.
2. doesnt show subscription.
3. it creates it without route