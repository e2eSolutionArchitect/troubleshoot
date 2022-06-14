Error creating DB Instance: InvalidParameterValue: The specified VPC is shared VPC, please explicitely provide an EC2 security group.

Context: You may have experienced this error while using Terraform for RDS creation in specific vpc. 

Resolution: Make sure you have "vpc_security_group_ids" defined in "aws_db_instance" resource. 
