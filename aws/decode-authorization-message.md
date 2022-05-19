

Ensure you have 'aws configure' done with valid access credentials.

### Step 1:
Check if you have StsDecode permission.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowStsDecode",
      "Effect": "Allow",
      "Action": "sts:DecodeAuthorizationMessage",
      "Resource": "*"
    }
  ]
}
```

### Step 2:
Decode the error message by below sts command

```
aws sts decode-authorization-message --encoded-message <copy-the encoded-message-here>
```
