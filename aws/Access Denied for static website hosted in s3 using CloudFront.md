Context: The static website is being hosted in a S3 bucket with ACL enabled. CloudFront is used and have setup with s3 origin. Route 53 is configured accordingly. 

Error:
Access Denied for static website hosted in s3 using CloudFront
<Error>
<Code>AccessDenied</Code>
<Message>Access Denied</Message>
<RequestId>34SDF54RTY4535</RequestId>
<HostId>
sdfsdf345345egfyty4567kjlhdsfs7sdfnw45wejhsdfsdf
</HostId>
</Error>

Resolution:
Could be many reasons. please [check here](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-ownership-migrating-acls-prerequisites.html?icmpid=docs_amazons3_console)

Quick things to check:
- CloudFront : Check 'Origin access'. It should be 'Legacy access identity' selected and Origin Access Identity selected from the drop-down. The s3 bucket policy should be updated for OAI
