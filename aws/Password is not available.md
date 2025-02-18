
# Password is not available

## While using the .pem file to decrypt password for the instance. The instance showing 'Password not available'. "Private key must begin with "------BEGIN RSA PRIVATE KEY-----" and end with "------END RSA PRIVATE KEY----""

## Solution

This is happensing for Metadata service IMDS v2. 
Select the instance > Actions > Instance settings > Metadata > Enable metadata.
Now try decrypting the password. 


If you are using terraforn then update 'metadata_options' as follows.
```
resource "aws_instance" "this" {
  ami                         = var.ami
  .......

  metadata_options {
    http_endpoint               = "enabled"
    http_tokens                 = "required"
    instance_metadata_tags      = "enabled"
    http_put_response_hop_limit = 1
  }

  tags                        = merge({ "Name" = var.name }, var.tags)
}

```
