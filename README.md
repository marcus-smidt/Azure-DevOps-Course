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
