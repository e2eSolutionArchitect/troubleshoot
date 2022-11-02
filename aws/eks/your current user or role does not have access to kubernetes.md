## your current user or role does not have access to kubernetes objects on this eks cluster

You might be logged in as administrator but you cant see your EKS resources in eks cluster. What!! Yes. It is not because of role. 
The EKS cluster control plane is not part of your AWS account or your VPC. That's why normally it doesn't have access unless you configure.
I got the great reference from [here](https://stackoverflow.com/questions/70787520/your-current-user-or-role-does-not-have-access-to-kubernetes-objects-on-this-eks)

Anyways now its our clean and clear step by step resolution here

1. 
