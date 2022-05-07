## EC2 instance is not showing in AWS Session Manager. 

### Resolution: The ec2 must have EC2 SSM role atatched. Check EC2 IAM role. 
In AWS Console goto Action > Security > Modify IAM Role

Secondly make sure the instance is at Running state and Status health check is 2/2 green

## NOTE: For Redhat 
Connect to other instance, SSH to the new EC2, 'scp' the file .rpm file to new ec2.
