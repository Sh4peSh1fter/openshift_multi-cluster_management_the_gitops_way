


* argocd-application-controller
    - k8s controller that continuously monitors running applications and compared the current live state against the desired target state.
* argocd-repo-server
    - internal service which maintains a local cache of the git repository holding the application manifests, and is responsible for generating and returning the k8s manifests.
* argocd-server
    - gRPC/REST server which exposes the api consumed by the web UI, CLI and CI/CD systems.