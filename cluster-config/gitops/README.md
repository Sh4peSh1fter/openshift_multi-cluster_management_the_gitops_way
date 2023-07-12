# openshift gitops operator - argocd

## the argocd dilema
you want an argocd instance to manage your applications with, thats obvious. but what if you need more? eigher if you have multiple environments or multiple application groups that want to be separetated to avoid "noisy neighbours" situation in the argocd instance. do you need more?

well there are few ways to manage your clusters and applications - in the perspective of structuring and managing your argocd instances:
1. main argo - one argo to manage them all.
2. main argo per cluster (env) - argo for each cluster.
3. main argo + sub argo per cluster (env) - sub argo for each cluster, and one main argo to manage all the sub argos.
4. main argo per workload type - argo for cluster-config, and argo for admin-apps.
5. main argo + sub argo per workload type - same with one main argo to manage them.

after diving in to the acm, maybe there is no need for a main argo to manage the other argos, when you have the acm that does that for you.


## best practices
1. Best Practices for Multi-tenancy in Argo CD by dan garfield
    * https://blog.argoproj.io/best-practices-for-multi-tenancy-in-argo-cd-273e25a047b0
2. ArgoCon 21 - Argo CD Production Best Practices by alexander matyushentsev
    * https://youtu.be/ESQLqjbM8h0


# sources
1. How to Kustomize your Codefresh/Argo Runtime by Ted Spinks
    * https://codefresh.io/blog/how-to-kustomize-your-codefresh-argo-runtime/
2. How many do you need? - Argo CD Architectures Explained
    * https://akuity.io/blog/argo-cd-architectures-explained/#:~:text=The%20single%20control%20plane%20approach,providing%20a%20great%20developer%20experience
3. ArgoCD Finalizer Shield: Protecting your clusters from unintended deletion
    * https://jfrog.com/community/cloud-native/argocd-protect-clusters-from-unintended-deletion/


# todo
1. should there be a "namespace" resource? all the other resources are created under the "openshift-gitops" namespace, yet it is not defined in our resources.
2. understand argo and its capabilities in depth, following https://argo-cd.readthedocs.io/en/stable/user-guide/.
