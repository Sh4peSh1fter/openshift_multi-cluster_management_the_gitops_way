# openshift storage

there are few terms you should know:
1. odf - openshift data foundation - 
2. ocs - openshit container storage - 
3. lso - local storage operators - 
4. mcg - multicloud object gateway - 
5. rook - 


## files explained
* odf-operator-sub.yaml - 
* odf-storage-cluster.yaml - 
* 


## cmd.sh
oc annotate namespace openshift-storage openshift.io/node-selector=
for i in `oc get nodes | grep inf | awk {'print $1'}`; do oc label node $i cluster.ocs.openshift.io/openshift-storage=; done
for i in `oc get nodes | grep inf | awk {'print $1'}`; do  oc adm taint node $i node.ocs.openshift.io/storage=true:NoSchedule; done