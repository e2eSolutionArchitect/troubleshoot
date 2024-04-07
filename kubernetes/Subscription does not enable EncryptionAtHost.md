## Subscription does not enable EncryptionAtHost


https://learn.microsoft.com/en-us/answers/questions/1377548/unable-to-enable-encryption-at-host-for-azure-vm

Register-AzProviderFeature -FeatureName "EncryptionAtHost" -ProviderNamespace "Microsoft.Compute"
Get-AzProviderFeature -FeatureName "EncryptionAtHost" -ProviderNamespace "Microsoft.Compute"
