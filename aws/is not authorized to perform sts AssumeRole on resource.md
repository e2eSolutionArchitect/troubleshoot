## An error occurred (AccessDenied) when calling the AssumeRole operation: User: arn:aws:iam::1111111111:user/myuser is not authorized to perform: sts:AssumeRole on resource: arn:aws:iam::1111111111:role/myrole

Fix:
Please refer [here](https://repost.aws/questions/QUOY5XngCtRyOX4Desaygz8Q/not-authorized-to-perform-sts-assumerole) for the AWS post

### Step 1: Create a policy to allow the action AssumeRole

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::1111111111:role/myrole"
        }
    ]
}
```

### Step 2: Attach the policy to the IAM user (myuser)
### Step 3: Update the trust policy of the role associated with the IAM user

Go to 'Trust Relationships' tab and add "AWS": "arn:aws:iam::1111111111:user/myuser"

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "eks.amazonaws.com",
                "AWS": "arn:aws:iam::1111111111:user/myuser"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```
