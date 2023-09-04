
# Subnets showing disabled while configuring Fargate profile for Amazon EKS

I know it is frustrating. Why all subnets are disabled!! 

Here I got the solution "The IDs of subnets to launch Pods into that use this profile. At this time, Pods that are running on Fargate aren't assigned public IP addresses. Therefore, only private subnets with no direct route to an Internet Gateway are accepted for this parameter." - [reference](https://docs.aws.amazon.com/eks/latest/userguide/fargate-profile.html)

So it is clear that you need a few PRIVATE subnets for the fargate profile. As your current subnets are all public that's why all are disabled for Fargate profile

Now follow the below steps to get the subnets enabled. 

- Create a route table under the same VPC
- Don't add Internet Gateway with the Route Table. It makes the RT private and any associated subnets will be private
- Go to the Subnet Association tab in that Route Table
- Edit the Subnet assignment and Assign the subnet(s) you want for the EKS Fargate profile.
- Make sure these subnets are not associated with any other public Route Table.
- That's it. Check the subnet list in the EKS Fargate profile config. You should be getting these subnets enabled. 
