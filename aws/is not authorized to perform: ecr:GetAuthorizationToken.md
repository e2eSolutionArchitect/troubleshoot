First probale reason is you dont have appropriate role. you need to have a permission "ecr:*" or specifically "ecr:GetAuthorizationToken".
Simply add "AmazonEC2ContainerRegistryFullAccess" role.

It is doesn't work it means your user has MFA enabled. You need to generate the AWS Token.

[Click here](https://github.com/e2eSolutionArchitect/kedb/blob/main/aws/generate%20aws%20session%20token.md) to generate AWS Token and now give a try. It should work now. 
