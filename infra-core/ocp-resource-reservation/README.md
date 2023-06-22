# resource management
## why
1. Provide more reliable scheduling
2. Minimize node resource overcommitment
3. Reserve resources (cpu and memory) for use by the underlying node components (such as kubelet and kube-proxy) and the system components (such as sshd and networkmanager).

## how
there are 2 ways to do so:
1. automatically determine the optimal system-reserved CPU and memory resources for your nodes.
2. manually determine and set the best resources for your nodes.

### automatically
1. Get the label associated with the machineconfigpool.
    - Run “oc edit machineconfigpool worker” and look at the “labels” section.
2. Create a CR (custom resource).
```yaml
apiVersion: machineconfiguration.openshift.io/v1
kind: KubeletConfig
metadata:
  name: dynamic-node 
spec:
  autoSizingReserved: true 
  machineConfigPoolSelector:
    matchLabels:
      pools.operator.machineconfiguration.openshift.io/worker: ""
```
    - Run “oc create -f <file_name>.yaml”
3. Verify that this was applied.
    - Run “oc debug node <node_name>”
    - Run “chroot /host”
    - View values in “/etc/node-sizing.env” 

### manually
the right way to calculate how much to allocate is described here: https://access.redhat.com/solutions/5843241

for example, if we want to calculate how much cpu to give our master node that got 8 cpu is:
6% of the first core.           - 60m
1% of the second core.          - 10m
0.5% of the next 2 cores.       - 5m
0.25% of any remaining core.    - 10m
so overall our node should get 85m cpu reservation.

# sources
1. Allocating resources for nodes in an OpenShift Container Platform cluster - https://docs.openshift.com/container-platform/4.10/nodes/nodes/nodes-nodes-resources-configuring.html
2. Openshift General Sizing - https://redhat-gitops-patterns.io/infrastructure/ocp-cluster-general-sizing/
3. Reserve Compute Resources for System Daemons - https://kubernetes.io/docs/tasks/administer-cluster/reserve-compute-resources/


---

# current resources
master - 8 cpu - 32g memory
worker - 8 cpu - 32g memory
infra - 16 cpu - 64g memory