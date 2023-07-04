# argocd application sets
## generators
what generatos should i use to create the application sets? well it depends on the structure you chose for your workload folder + the way you decided to devide your application sets (app per what).

## sync options
what sync options should i define on the application sets.

## things to make sure
1. make sure importent applications won't have thier resources deleted. can be done with correct labels.
2. 


# sources
1. An Introduction to ApplicationSets in OpenShift GitOps
    * https://cloud.redhat.com/blog/an-introduction-to-applicationsets-in-openshift-gitops

# todo
1. should i add a "namespace" field in the kustomization yaml?
2. should i crete a helm chart from this directory so i can insert values for the application sets yaml, such as the repo url?