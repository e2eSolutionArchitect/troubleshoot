# EC2 Metadata not accessible

For security reason IMDS should be enavled to V2. Which is restrict ec2 metadata to access. Please follow below steps to access metadata when using IMDS V2.

# For IMDS V2 - use token for metadata access
# Get the session token
$token = Invoke-RestMethod -Uri "http://169.254.169.254/latest/api/token" -Method Put -Headers @{ "X-aws-ec2-metadata-token-ttl-seconds" = "21600" } 
# Access metadata using the session token
$InstanceId = Invoke-RestMethod -Uri "http://169.254.169.254/latest/meta-data/instance-id" -Headers @{ "X-aws-ec2-metadata-token" = $token } 
$AZ = Invoke-RestMethod -Uri "http://169.254.169.254/latest/meta-data/placement/availability-zone" -Headers @{ "X-aws-ec2-metadata-token" = $token }

# Get the instance's private IP address
$PrivateIP = Invoke-RestMethod -Uri "http://169.254.169.254/latest/meta-data/local-ipv4" -Headers @{ "X-aws-ec2-metadata-token" = $token } 
$PublicIP = Invoke-RestMethod -Uri "http://169.254.169.254/latest/meta-data/public-ipv4" -Headers @{ "X-aws-ec2-metadata-token" = $token }


Note: It is good to have IMDS V2 is forced at account level. to do that pelase run below command and check the IMDS status. 

Edit-EC2InstanceMetadataDefault -Region $Region -HttpToken "required" -HttpPutResponseHopLimit 2 -InstanceMetadataTags "enabled"
Get-EC2InstanceMetadataDefault -Region $Region | Format-List
