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
**Creating Azure RGs with VMs (Linux and Windows)**
```bash
$ az group create --name practice5task3_azrg --location eastus
{
  "id": "/subscriptions/xxxxx-x-x-x-xxxxx/resourceGroups/practice5task3_azrg",
  "location": "eastus",
  "managedBy": null,
  "name": "practice5task3_azrg",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}
```
```bash
$ az vm create --resource-group practice5task3_azrg --name MyVM1 --image Ubuntu2204 --admin-username azureuser --admin-password "xxxxxx" # or --generate-ssh-keys
{
  "fqdns": "",
  "id": "/subscriptions/xxxxx-x-x-x-xxxxx/resourceGroups/practice5task3_azrg/providers/Microsoft.Compute/virtualMachines/MyVM1",
  "location": "eastus",
  "macAddress": "00-22-48-32-D2-78",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "172.191.208.x",
  "resourceGroup": "practice5task3_azrg",
  "zones": ""
}
```
```bash
$ New-AzResourceGroup -Name practice5task3_pshrg -Location "East US"

ResourceGroupName : practice5task3_pshrg
Location          : eastus
ProvisioningState : Succeeded
Tags              : 
ResourceId        : /subscriptions/xxxxx-x-x-x-xxxxx/resourceGroups/practice5task3_pshrg
```
```bash
$ New-AzVm -ResourceGroupName practice5task2_pshrg -Name MyVM2 -Location "eastus" -ImageName "MicrosoftWindowsServer:WindowsServer:2022-datacenter:latest" -VirtualNetworkName "MyVNet" -SubnetName "MySubnet" -SecurityGroupName "MyNSG" -PublicIpAddressName "MyPublicIP" -OpenPorts 3389 

cmdlet New-AzVM at command pipeline position 1
Supply values for the following parameters:
Credential
User: azureuser
Password for user azureuser: ****************
No Size value has been provided. The VM will be created with the default size Standard_D2s_v3.
                                                                                                                        
                                                                                                                        
ResourceGroupName        : practice5task2_pshrg                                                                         
Id                       : /subscriptions/xxxxx-x-x-x-xxxxx/resourceGroups/practice5task2_pshrg/providers/Micros
oft.Compute/virtualMachines/MyVM2                                                                                       
VmId                     : cea3d153-1c78-471b-8201-d1c8e6756411                                                         
Name                     : MyVM2                                                                                        
Type                     : Microsoft.Compute/virtualMachines                                                            
Location                 : eastus                                                                                       
Tags                     : {}                                                                                           
HardwareProfile          : {VmSize}                                                                                     
NetworkProfile           : {NetworkInterfaces}                                                                          
OSProfile                : {ComputerName, AdminUsername, WindowsConfiguration, Secrets, AllowExtensionOperations,       
RequireGuestProvisionSignal}                                                                                            
ProvisioningState        : Succeeded                                                                                    
StorageProfile           : {ImageReference, OsDisk, DataDisks}                                                          
FullyQualifiedDomainName : myvm2-81933e.eastus.cloudapp.azure.com                                                       
TimeCreated              : 2/7/2025 4:15:09 PM
```
![Screenshot](screenshots_task3/vms_overview.png)


**Retrieving VMs details using CLI**
```bash
$ az vm show --resource-group practice5task3_azrg --name MyVM1 --output table
$ Get-AzVm -ResourceGroupName practice5task3_pshrg -Name MyVM2
```

**Stopping and deleting Azure VMs**
```bash
$ az vm stop --resource-group practice5task3_azrg --name MyVM1
$ Stop-AzVm -ResourceGroupName practice5task3_pshrg -Name MyVM2 -Force
$ az vm delete --resource-group practice5task3_azrg --name MyVM1 --yes
$ Remove-AzVm -ResourceGroupName practice5task3_pshrg -Name MyVM2 -Force
```





