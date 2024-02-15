## cannot import name 'csv_serializer' from 'sagemaker.predictor'

## Resolution

Execute below. Please [check reference here](https://github.com/aws/amazon-sagemaker-examples/issues/1372)
```
pip install sagemaker==1.72.0
```
