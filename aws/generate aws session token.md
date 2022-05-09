once you have MFA enabled you have to pass the AWS session token to execute aws cli commands. 

I assume you have "aws configure" d already

Step 1: go to your home profile

cd ~ or cd /home/ec2-user/
ls -a  (to see hidden directories. you must see .aws folder if you have aws cli configuied. Otherwise check here to configure AWS CLI)

Step 2:
```
aws sts get-session-token --serial-number arn:aws:iam::<aws account number>:mfa/<username> --token-code ###### (###### enter your auth code from authenticator for this user)

aws sts get-session-token --serial-number arn:aws:iam::9647723:mfa/myuser --token-code 847855
```
it will show output with three attributes AccessKeyId, SecretAccessKey, SessionToken. copy the values and map as follows

```
[default]
aws_access_key_id= <AccessKeyId>
aws_secret_access_key=<SecretAccessKey>
aws_session_token=<SessionToken>

```
Step 3:
Copy abive 4 lines of code in below 'credentials' file and save
```
sudo vi .aws/credentials
```
