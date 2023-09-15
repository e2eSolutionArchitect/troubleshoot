## Disabled logging because aws-logging configmap was not found. configmap "aws-logging" not found
You might be deploying pods in EKS Fargate and received this error. 
It is because of cloudwatch configuration and permission. 

Please refer this AWS documentation [click here](https://docs.aws.amazon.com/eks/latest/userguide/fargate-logging.html)

Step 1: Create below IAM Policy and attach it with PoD Execote Role
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogStream",
                "logs:CreateLogGroup",
                "logs:DescribeLogStreams",
                "logs:PutLogEvents"
            ],
            "Resource": "*"
        }
    ]
}

```

Step 2: Add ConfigMap to your deployment yml as below. Make sure you have same namespace and correct region-code. region us-east-1

```
---
apiVersion: v1
kind: Namespace
metadata:
  name: cav-ns
  labels:
    aws-observability: enabled
---
kind: ConfigMap
apiVersion: v1
metadata:
  name: aws-logging
  namespace: cav-ns
data:
  flb_log_cw: "true" 
  filters.conf: |
    [FILTER]
        Name parser
        Match *
        Key_name log
        Parser crio
    [FILTER]
        Name kubernetes
        Match kube.*
        Merge_Log On
        Keep_Log Off
        Buffer_Size 0
        Kube_Meta_Cache_TTL 300s
  output.conf: |
    [OUTPUT]
        Name cloudwatch_logs
        Match   kube.*
        region us-east-1
        log_group_name my-logs
        log_stream_prefix from-fluent-bit-
        log_retention_days 60
        auto_create_group true
  parsers.conf: |
    [PARSER]
        Name crio
        Format Regex
        Regex ^(?<time>[^ ]+) (?<stream>stdout|stderr) (?<logtag>P|F) (?<log>.*)$
        Time_Key    time
        Time_Format %Y-%m-%dT%H:%M:%S.%L%z

```

Step 3: 

If still it shows the same issue it means your pods are trying to reach public internet but failing. So make sure you have NAT Gateway enabled. Natgateway should be created in PUBLIC subnet and natgw should be attached to the PRIVATE RouteTable. 

```
  NATGateway:
    Type: AWS::EC2::NatGateway
    Properties:
        AllocationId: !GetAtt NATGatewayEIP.AllocationId
        SubnetId: !Ref PublicSubnet
        Tags:
        - Key: env
          Value: dev
  NATGatewayEIP:
    Type: AWS::EC2::EIP
    Properties:
        Domain: vpc
  RouteNATGateway:
    DependsOn: NATGateway
    Type: AWS::EC2::Route
    Properties:
        RouteTableId: !Ref PrivateRouteTable
        DestinationCidrBlock: '0.0.0.0/0'
        NatGatewayId: !Ref NATGateway

```
