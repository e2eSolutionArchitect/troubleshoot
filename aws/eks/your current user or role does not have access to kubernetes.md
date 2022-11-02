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
