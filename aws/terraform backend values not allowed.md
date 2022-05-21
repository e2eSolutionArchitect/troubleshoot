## Error: terraform backend values not allowed
You may have experienced this error while trying to pass params in terraform backend like below 

```
  backend "s3" {
    bucket         = var.backend_s3_bucket
    key            = "${var.project}/terraform.tfstate"
    region         = var.aws_region
    dynamodb_table = var.backend_dynamodb_table
    encrypt        = true
    profile        = var.profile
  }
```

Being terraform develoers we hate to hard code values. Then what's the solution??

Here goes the workaround which is little long but it saves you from hardcoding and managing multiple environments (dev,int,stg,prd) in parallel.

## Solution

### Step 1:

versions.tf  should look like below. with empty backend 

```
terraform {
  required_version = "~> 1.1.6"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.5.0"
    }
  }
  backend "s3" {}
}

# provider block

provider "aws" {
  profile = var.profile
  region  = var.aws_region
}

```
