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
4. the ns is stuck on terminating.
most likely this should resolve it:
    * oc patch -n openshift-gitops appproject.argoproj.io/admin-apps-project --type=merge -p '{"metadata": {"finalizers":null}}'
    * oc patch -n ${NAMESPACE} appproject.argoproj.io/cluster-configs-project --type=merge -p '{"metadata": {"finalizers":null}}'
it may return. i tried oc delete ns/openshift-gitops and then kubectl delete -k bootstrap/overlays/hub-cluster/ and it didnt work.
    * oc delete csv openshift-gitops-operator.v1.5.10 -n openshift-operators
    * oc delete ns/openshift-gitops


# sources
1. https://github.com/rhilconsultants/Application-Deployment-Workshop/tree/main
2. https://github.com/rhilconsultants/HELM-ArgoCD-Lab/tree/main
3. https://developers.redhat.com/articles/2023/05/25/3-patterns-deploying-helm-charts-argocd#
4. https://github.com/argoproj/argocd-example-apps/tree/master/plugins/kustomized-helm
5. https://levelup.gitconnected.com/from-local-development-to-kubernetes-cluster-helm-https-ci-cd-gitops-kustomize-argocd-5d9de6f5364c
6. https://trstringer.com/helm-kustomize/
7. https://github.com/mathieu-benoit/ci-with-helm/tree/main
8. https://github.com/fluxcd/flux2-kustomize-helm-example

