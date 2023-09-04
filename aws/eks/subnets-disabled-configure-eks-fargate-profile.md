
# Subnets showing disabled while configuring Fargate profile for Amazon EKS
I know it is frustating. Why all subnets are disabled!! 
Here I got the solution "The IDs of subnets to launch Pods into that use this profile. At this time, Pods that are running on Fargate aren't assigned public IP addresses. Therefore, only private subnets with no direct route to an Internet Gateway are accepted for this parameter." - [reference](https://docs.aws.amazon.com/eks/latest/userguide/fargate-profile.html)
