## could not delete cluster cluster has nodegroups attached

This could be an irritating issue. It can happen for many reasons. Please find following steps to debug

Initial quick fix: 
- Go to security groups and find security group named as "eks-remoteAccess-". delete it manually. 
- If it restrict to delete that means it has a network interface "eni-######" associated with it.
- To delete the network interface, search and open network interface on another brower tab and simply delete the particular ENI "eni-######". Now go back to security group brower tab and try deleting the  security group named as "eks-remoteAccess-". It should delete now. 
- Now go back to EKS cluster Node group screen and select the nodegroup and click delete. 
- Now select the cluster and click delete.

If above steps doesnt help then be ready for a deep dive. 

- Are you able to view EKS cluster resources in AWS management console? if No please [check here](https://github.com/e2eSolutionArchitect/KEDB/blob/main/aws/eks/your%20current%20user%20or%20role%20does%20not%20have%20access%20to%20kubernetes.md) to follow the step to make them accessible in your EKS portal. 
- Once the resources are visible follow below steps
  - select the cluster and go to 'compute' tab
  - you will find list of node groups
  - select the node group and click delete manually.
  - If it doesnt delete. Go inside the node group and in "Message" tab you will find the reason for failure. 
- Secondly you run kubectl command to delete the nodes and cluster. [click here](https://github.com/e2eSolutionArchitect/scripts/blob/main/kubernetes/k8s-handy-commands.md) for the commands. 
