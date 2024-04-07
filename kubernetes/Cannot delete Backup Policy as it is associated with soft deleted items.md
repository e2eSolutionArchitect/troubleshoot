https://learn.microsoft.com/en-us/answers/questions/599385/cannot-delete-backup-vault
You can't delete a vault that contains backup data in the soft deleted state.

Cannot delete Backup Policy as it is associated with soft deleted items
Select backup valut > select 'Backup policy' > Datasource Type = 'Kubernetes Services' (by default is it not selected). It should show the backup policy and delete the backup policy manually. 

Recovery Services vault cannot be deleted as there are backup items in soft deleted state in the vault. The soft deleted items are permanently deleted after 14 days of delete operation.
