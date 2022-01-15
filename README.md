# Can't find my node!
The sad story of a PVC that can't find his node.<br>
How an annotation in a pvc can prevent a pod to be recreated in another node.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f3/Lost_main_title.svg/800px-Lost_main_title.svg.png" alt="LostImage"  width="50%" height="50%">

<br><br>
Steps:<br>
1 - Create a cluster in AKS without autoscaler<br>
2 - Create a PVC<br>
3 - Insert an anotation to the PVC to force it to belong to a specific node<br>
4 - Create a deployment with a claim and mount configured<br>
5 - Scale the vmss to zero<br>
6 - Scale the vmss to one<br>
7 - Check the pod lost without PVC<br>
8 - Remove the PVC annotation and wait the pod to be alive<br>
9 - Delete everything<br>
<br><br>
1 -<br>
az group create --name lostnode --location eastus<br>
az aks create --resource-group lostnode --name k8snoautoscaler --node-count 1 --generate-ssh-keys<br>
2 -<br>
kubectl apply -f https://raw.githubusercontent.com/zectorpt/cantfindmynode/main/pvc.yaml<br>
3 -<br>
kubectl get nodes<br>
kubectl annotate persistentvolumeclaims manageddisk volume.kubernetes.io/selected-node=[NODENAME]<br>
4 -<br>
kubectl apply -f https://raw.githubusercontent.com/zectorpt/cantfindmynode/main/deployment.yaml<br>
5 -<br>
az vmss list -o table -g MC_lostnode_k8snoautoscaler_eastus
az vmss scale --name aks-nodepool1-33463102-vmss --new-capacity 0 --resource-group MC_lostnode_k8snoautoscaler_eastus<br>
6 -<br>
az vmss scale --name aks-nodepool1-33463102-vmss --new-capacity 1 --resource-group MC_lostnode_k8snoautoscaler_eastus<br>
7 -<br>
kubectl get pods<br>
kubectl describe pod ubuntu-[PODNAME]<br>
8 -<br>
kubectl annotate persistentvolumeclaims manageddisk volume.kubernetes.io/selected-node-<br>
9 -<br>
az group delete --name lostnode
