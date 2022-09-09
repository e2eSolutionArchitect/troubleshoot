
Reference : https://aws.amazon.com/premiumsupport/knowledge-center/ec2-not-auth-launch/#:~:text=The%20%22UnauthorizedOperation%22%20error%20indicates%20that,permissions%20to%20launch%20EC2%20instances.

Decode the message first 
```
aws sts decode-authorization-message --encoded-message encoded-message
```

If the decoded message is similar to below message then add "iam:PassRole". 

"DecodedMessage": 
"{\"allowed\":false,\"explicitDeny\":false,\"matchedStatements\":{\"items\":[]},\"failures\":{\"items\":[]},\"context\":{\"principal\":{\"id\":\"ABCDEFGHIJKLMNO\",\"name\":\"AWS-User\",
\"arn\":\"arn:aws:iam::accountID:user/test-user\"},\"action\":\"iam:PassRole\",
\"resource\":\"arn:aws:iam::accountID:role/EC2_instance_Profile_role\",\"conditions\":{\"items\":[{\"key\":\"aws:Region\",\"values\":{\"items\":[{\"value\":\"us-east-2\"}]}},
{\"key\":\"aws:Service\",\"values\":{\"items\":[{\"value\":\"ec2\"}]}},{\"key\":\"aws:Resource\",\"values\":{\"items\":[{\"value\":\"role/EC2_instance_Profile_role\"}]}},
{\"key\":\"iam:RoleName\",\"values\":{\"items\":[{\"value\":\"EC2_instance_Profile_role\"}]}},{\"key\":\"aws:Account\",\"values\":{\"items\":[{\"value\":\"accountID\"}]}},
{\"key\":\"aws:Type\",\"values\":{\"items\":[{\"value\":\"role\"}]}},{\"key\":\"aws:ARN\",\"values\":{\"items\":[{\"value\":\"arn:aws:iam::accountID:role/EC2_instance_Profile_role\"}]}}]}}}"
}

"The preceding error message indicates that the request failed to call RunInstances because AWS-User didn't have permission to perform the iam:PassRole action on the arn:aws:iam::accountID:role/EC2_instance_Profile_role." -- copied from AWS reference
