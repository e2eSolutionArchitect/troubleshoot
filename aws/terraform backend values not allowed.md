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

### Step 2:

create a config file 'backend.config'. if you are managing multiple environments in parallel then you can create multiple files like <env>-backend.config (dev-backend.confi, prd-backend.config)

<env>-backend.config contains your environment specific back details like below
  
```
    bucket         = "e2esa-tf-states"
    key            = "ecs-cluster/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "e2esa-tf-locks"
    encrypt        = true
    profile        = "development"
  
```
  
### Step 3:

Now initialize and apply terraform
### IMPORTANT NOTE
  If you are managing SINGLE environment only then don't worry 
  
  ```
  terraform init -backend-config="backend.conf"   
  ```
  
  If you are managing MULTIPLE environments (dev,qa,prd etc) then you have to -reconfigure EVERY TIME you want to switch environment like below
  
  ```
  Initialize very first time (suppose for dev environment)
  
  terraform init -backend-config="dev-backend.conf" 
  terraform apply
  
  Now switching to other environemnt (suppose prod )
  
  terraform init -reconfigure -backend-config="prod-backend.conf" 
  terraform apply
  
  NEVER do -migrate-state. Otherwise migrate-state will actually initialize your Prod backend with Dev state and will mixup
  
  Now again switch to another environment (Suppose QA)
  
  terraform init -reconfigure -backend-config="qa-backend.conf" 
  terraform apply
  
  And enjoy managing multiple environments like this
  ```
  
  A step by step video tutorial will be shared soon.
  for any queries please feel free to contact som@e2eSolutionArchitect.com and visit Terraform articles here https://e2esolutionarchitect.com/tag/terraform/
  
  [Click here](https://www.youtube.com/watch?v=v3M_PJAcpzU&list=PLuBBTh-4TzDkUiWqlrwwnJ3QFJdP4JiPy) for Terraform Advance tutorials
  
