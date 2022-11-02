### The connection to the server localhost:8080 was refused - did you specify the right host or port?

It means the kube config is missing
```
aws eks --region <region> update-kubeconfig --name <cluster-name>
aws eks --region us-east-1 update-kubeconfig --name e2esa-tutorials-eks-cluster
```

you can run the above command from your local or bastian host. but the system should have awscli installed. 
To know how to install awscli please [check here](https://github.com/e2eSolutionArchitect/scripts/blob/main/aws/ec2/awscli-install.md)


After kubeconfig you should be able to execute following commands from your system 

```
kubectl get nodes
kubectl get nodes -o wide
kubectl get svc
```
