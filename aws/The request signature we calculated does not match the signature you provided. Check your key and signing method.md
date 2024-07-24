
## The request signature we calculated does not match the signature you provided. Check your key and signing method

```
Due to missing header

headers = {'Bucket': bucket, 'Key': key}
        response = requests.put(presigned_url, data=content_decoded, headers=headers)

```

Because we created the presigned url using 

```
        presigned_url = s3.generate_presigned_url('put_object', Params={'Bucket': bucket, 'Key': key}, ExpiresIn=360 )
```
headers needs to match with params. 
```
However, Adding metadata to header didn't work

headers = {'Bucket': bucket, 'Key': key}
```     
