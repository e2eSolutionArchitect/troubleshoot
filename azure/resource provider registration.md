
# Resource Provider Registration [click here](https://learn.microsoft.com/en-us/azure/azure-resource-manager/troubleshooting/error-register-resource-provider?tabs=azure-cli)

Cause
- You receive these errors for one of these reasons:
- The required resource provider hasn't been registered for your subscription.
- API version not supported for the resource type.
- Location not supported for the resource type.
- For VM auto-shutdown, the Microsoft.DevTestLab resource provider must be registered.


```
az provider list --output table
az provider list --query "[?registrationState=='Registered']" --output table
az provider list --query "[?Namespace=='Microsoft.ImportExport']" --output table

# register new resource provider
az provider register --namespace Microsoft.Cdn
```
