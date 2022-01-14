# Can't find my node!
The sad story of a PVC that can't find his node.

Reproduction of:
1 - Create a cluster in AKS without autoscaler
2 - Create a PVC
3 - Insert an anotation to the PVC to force it to belong to a specific node
4 - Create a pod with the claim and mount configured
5 - Scale the vmss to zero
6 - Scale the vmss to one
7 - Check the pod lost without PVC
8 - Remove the PVC annotation and wait the pod to be alive

