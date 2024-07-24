
## Requests specifying Server Side Encryption with AWS KMS managed keys require AWS Signature Version 4

```
s3 = boto3.client('s3')
```

Updated to 

```

from botocore.config import Config

config = Config(signature_version='s3v4')
s3 = boto3.client('s3', config=config)

```
