
Access to the path '/opt/vsts/ado_agent/_work/4/a/.terraform' is denied

This error came up while running a AzureDevOps pipeline to call to kickoff terraform which is running in an AWS VM on docker. 
Basically terraform is running on docker in AWS VM and from Azure DevOps the pipeline trying to kickoff the terraform pipeline. 

Reason of this issue is, The pipeline always clean the work folder before it starts. First time when the pipeline starts it gets everything empty and the pipeline runs.
But next time onwards it cleans workfolder everytime and then run the pipeline. 

The user which run the ADO pipeline is "vsts". It doesn't support to run 'sudo'

Normally login to the VM , the path "/opt/vsts/ado_agent/_work/4/a/" is not available. For that follow the below steps

-
