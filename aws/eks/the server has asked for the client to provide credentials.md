## couldn't get current server API group list: the server has asked for the client to provide credentials

Configure k8s cluster in bastion instance or your local system where kubectl is installed 

```
aws eks --region us-east-1 update-kubeconfig --name e2esa-demo-eks-cluster

wget https://s3.us-west-2.amazonaws.com/amazon-eks/docs/eks-console-full-access.yaml
```
```
#Please make sure you have AWS CLI configured in the system

kubectl apply -f eks-console-full-access.yaml
```

Update configmap
```
kubectl edit configmap aws-auth -n kube-system
```

configmap to allow an ec2 bastion host to connect to cluster by role "e2esa-ec2-profile-bastion-role".
Goal is to allow any user who connects through bastion host should be able to connect eks clsuter without using user's aws creds

```
apiVersion: v1
data:
  mapRoles: |
    - groups:
      - system:bootstrappers
      - system:nodes
      rolearn: arn:aws:iam::<aws-acc-no>:role/e2esa-eks-nodegroup-role
      username: system:node:{{EC2PrivateDNSName}}
    - rolearn: arn:aws:iam::<aws-acc-no>:role/e2esa-eks-cluster-access-role
      username: eksadmin
      groups:
      - system:masters
    - rolearn: arn:aws:iam::<aws-acc-no>:role/e2esa-ec2-profile-bastion-role
      username: ec2bastionusr
      groups:
      - system:masters
  mapUsers: |
    - userarn: arn:aws:iam::<aws-acc-no>:user/<iam-user-name>
      username: <iam-user-name>
      groups:
      - system:bootstrappers
      - system:nodes
	  - system:masters
      - eks-console-dashboard-full-access-group
kind: ConfigMap
metadata:
  creationTimestamp: "2022-11-02T19:37:33Z"
  name: aws-auth
  namespace: kube-system
  resourceVersion: "730"
  uid: 1cghhtf0-bgh3-4ed1-b7df-e9dd7310oj7fd
```

multiple user can be added like below
```
    mapUsers: |
    - userarn: arn:aws:iam::<aws-acc-no>:user/<iam-user-name>
      username: <iam-user-name>
      groups:
      - e2esa-development-role-01 # Role from K8S Role contructs
      - system:bootstrappers
      - system:nodes
      - eks-console-dashboard-full-access-group

```
