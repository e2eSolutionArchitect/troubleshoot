# Error "Unable to import module index No module named index."

## When: While testing an Amazon Lambda Function manually from aws console. Same issue for multiple lambda functions. 

```
{
  "errorMessage": "Unable to import module 'index': No module named 'index'",
  "errorType": "Runtime.ImportModuleError",
  "requestId": "",
  "stackTrace": []
}
```

## Root Cause:

AWS Lambda's default handler is often set to index.lambda_handler in Python runtimes, meaning it expects a file named index.py and a function within it named lambda_handler

## Resolution

- make `lambda_handler   = "index.lambda_handler"`
- make `function_file    = "../lambda/event-handler-lambda/index.py"`
- ensure index.py has `def lambda_handler(event, context):`
