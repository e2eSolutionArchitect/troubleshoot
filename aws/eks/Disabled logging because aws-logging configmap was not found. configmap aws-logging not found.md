## Disabled logging because aws-logging configmap was not found. configmap "aws-logging" not found
You might be deploying pods in EKS Fargate and received this error. 
It is because of cloudwatch configuration and permission. 

Please refer this AWS documentation [click here](https://docs.aws.amazon.com/eks/latest/userguide/fargate-logging.html)

Step 1: Add ConfigMap to your deployment yml as below. Make sure you have same namespace and correct region-code. region us-east-1

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
