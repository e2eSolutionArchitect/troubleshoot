## your current user or role does not have access to kubernetes objects on this eks cluster

You might be logged in as administrator but you cant see your EKS resources in eks cluster. What!! Yes. It is not because of role. 
The EKS cluster control plane is not part of your AWS account or your VPC. That's why normally it doesn't have access unless you configure.
I got the great reference from [here](https://stackoverflow.com/questions/70787520/your-current-user-or-role-does-not-have-access-to-kubernetes-objects-on-this-eks)

Anyways now its our clean and clear step by step resolution here

1. I assume you have a system/bastian host configured with awscli and kubectl. connect to that system and run below commands to update kubeconfig

```
ubuntu@ip-172-31-44-3:~$ aws eks --region us-east-1 update-kubeconfig --name e2esa-tutorials-eks-cluster
Added new context arn:aws:eks:us-east-1:<aws-acc-id>:cluster/<cluster-name> to /home/ubuntu/.kube/config

Now test with below commands


ubuntu@ip-172-31-44-3:~$ kubectl get nodes
NAME                            STATUS   ROLES    AGE    VERSION
ip-172-31-45-158.ec2.internal   Ready    <none>   133m   v1.23.9-eks-ba74326

ubuntu@ip-172-31-44-3:~$ kubectl get nodes -o wide
NAME                            STATUS   ROLES    AGE    VERSION               INTERNAL-IP     EXTERNAL-IP      OS-IMAGE         KERNEL-VERSION                 CONTAINER-RUNTIME
ip-172-31-45-158.ec2.internal   Ready    <none>   134m   v1.23.9-eks-ba74326   172.31.45.158   54.165.160.164   Amazon Linux 2   5.4.209-116.367.amzn2.x86_64   docker://20.10.17

ubuntu@ip-172-31-44-3:~$ kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   172.20.0.1   <none>        443/TCP   139m
```
2. You need to Create a cluster role and cluster role binding.. Reference [click here](https://aws.amazon.com/premiumsupport/knowledge-center/eks-kubernetes-object-access-error/)
For that, 
2.1 Download the manifest file from https://s3.us-west-2.amazonaws.com/amazon-eks/docs/eks-console-full-access.yaml
nothing to update in this file. It has all the priviledges configured.

```
wget https://s3.us-west-2.amazonaws.com/amazon-eks/docs/eks-console-full-access.yaml
```
Make sure you have aws configure done. 

2.2 Deploy the manifest file:

kubectl apply -f eks-console-full-access.yaml

```
ubuntu@ip-172-31-44-3:~$ kubectl apply -f eks-console-full-access.yaml

output
clusterrole.rbac.authorization.k8s.io/eks-console-dashboard-full-access-clusterrole created
clusterrolebinding.rbac.authorization.k8s.io/eks-console-dashboard-full-access-binding created
```

2.3   Update your aws-auth ConfigMap with the new group eks-console-dashboard-full-access-group for your IAM entity:
The below command popup a config file. Add the user "mapUsers" portion mentioned below in the mentioned format
```
kubectl edit configmap aws-auth -n kube-system
```

```
apiVersion: v1
data:
-------------start copy---------------- 
  mapUsers: |
    - userarn: arn:aws:iam::<aws-acc-id>:user/<aws-username>
      username: <aws-username>
      groups:
      - system:bootstrappers
      - system:nodes
      - eks-console-dashboard-full-access-group
 -------------end copy---------------- 
  mapRoles: |
    - groups:
      - system:bootstrappers
      - system:nodes
      rolearn: arn:aws:iam::<aws-acc-id>:role/e2esa-tutorials-eks-nodegroup-role
      username: system:node:{{EC2PrivateDNSName}}
kind: ConfigMap
metadata:
  creationTimestamp: "2022-11-02T19:37:33Z"
  name: aws-auth
  namespace: kube-system
  resourceVersion: "730"
  uid: 566ghgh-56ht-4cc1-jyr-67454hfgfg

```

```
ubuntu@ip-172-31-44-3:~$ kubectl edit configmap aws-auth -n kube-system

output 
configmap/aws-auth edited
```

3. refresh the cluster page and you should be able to see all the resources under the cluster.
