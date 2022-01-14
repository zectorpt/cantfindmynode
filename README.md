# Can't find my node!
The sad story of a PVC that can't find his node.
<br><br>
Reproduction of:<br>
1 - Create a cluster in AKS without autoscaler<br>
2 - Create a PVC<br>
3 - Insert an anotation to the PVC to force it to belong to a specific node<br>
4 - Create a pod with the claim and mount configured<br>
5 - Scale the vmss to zero<br>
6 - Scale the vmss to one<br>
7 - Check the pod lost without PVC<br>
8 - Remove the PVC annotation and wait the pod to be alive<br>

