# Azure DevOps Course tasks results (Practice #5)

## Task 1
**Checking if az cli and PowerShell are installed on the host Ubuntu machine**
```bash
$ az --version
$ az login
$ az account show #json output
```
```bash
$ Get-Module -Name Az -ListAvailable
$ Connect-AzAccount
$ Get-AzSubscription
```
```bash
salmon@xxxxx:~/Desktop/xxxxx/Azure-DevOps-Course$ az account show
{
  "environmentName": "AzureCloud",
  "homeTenantId": "xxxxx-x-x-x-xxxxx",
  "id": "xxxxx-x-x-x-xxxxx",
  "isDefault": true,
  "managedByTenants": [],
  "name": "Azure subscription 1",
  "state": "Enabled",
  "tenantId": "xxxxx-x-x-x-xxxxx",
  "user": {
    "name": "Markiianxxxxx@xxxxxgmail.onmicrosoft.com",
    "type": "user"
  }
}
```

## Task 2
**Azure Resource Group creation with both az cli and Powershell**
```bash
$ az group create --name practice5task2_az --location eastus
{
  "id": "/subscriptions/xxxxx-x-x-x-xxxxx/resourceGroups/practice5task2_az",
  "location": "eastus",
  "managedBy": null,
  "name": "practice5task2_az",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}
```
```bash
$ New-AzResourceGroup -Name practice5task2_psh -Location "East US"
ResourceGroupName : practice5task2_psh
Location          : eastus
ProvisioningState : Succeeded
Tags              : 
ResourceId        : /subscriptions/xxxxx-x-x-x-xxxxx/resourceGroups/practice5task2_psh
```
**Listing Azure Resource Groups from the CLI**
```bash
$ az group list --output table
$ Get-AzResourceGroup
```

**For convenience purposes bash and powershell terminals were created in VS Code**
![Screenshot](screenshots_task2/vs-code-overview.png)

**Azure Resource Groups cleaning up from the CLI**
```bash
$ az group delete --name practice5task2_az --yes --no-wait
$ Remove-AzResourceGroup -Name practice5task2_psh -Force #output: True
```

## Task 3



