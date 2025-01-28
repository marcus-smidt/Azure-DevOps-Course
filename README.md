# Azure DevOps Course tasks results (Practice #3)
## Task 1

**Azure Storage Account and Blob container created**
![ScreenShot](screenshots_task1/Blob%20container%20overview.png)

**Storage Exlorer installed on Ubuntu 22 localhost machine**
![ScreenShot](screenshots_task1/Storage%20Explorer%20view.png)

**Sample file uploaded via Azure Portal**
![ScreenShot](screenshots_task1/File%20upload%20into%20Blob%20container.png)

**Sample file retrieval (downloading on the local machine)**
![ScreenShot](screenshots_task1/Sample%20file%20retrieval.png)

**New file added locally via Storage Explorer**
![ScreenShot](screenshots_task1/New%20file%20uploaded%20(1984).png)


## Task 2
**Sample files uploaded**
![ScreenShot](screenshots_task2/sample%20files%20uploaded.png)

**Data Management Lifecycle Rule added**
![ScreenShot](screenshots_task2/lifecycle-rule.png)

**Moving to cool access tier after 0 days (for testing purposes, IRL would be closer to 14-30 days)**
![ScreenShot](screenshots_task2/movetocool0.png)

**Blob filtering with full path or prefix naming (blob indexing also as an option)**
![ScreenShot](screenshots_task2/blob-filtering.png)

**Different tiers overview (difference between accessing and storage costs)**
![ScreenShot](screenshots_task2/tier%20changed.png)

## Task 3
**Azure Queue service created via an Azure Portal**
![ScreenShot](screenshots_task3/queue%20created.png)

**Azure Queue populated with the sample message**
![ScreenShot](screenshots_task3/message-created.png)

**Message content reviewing inside Azure Storage Explorer on the local machine**
![ScreenShot](screenshots_task3/queue%20message%20review.png)

**Testing multiple messages requeuing (Azure Storage Explorer)**
![ScreenShot](screenshots_task3/messages-requeued.png)

**Testing multiple message dequeuing (Azure Storage Explorer)**
![ScreenShot](screenshots_task3/messages-dequeued.png)

## Task 4
**Prerequisites commands executed**
```bash
sudo apt-get install keyutils 

sudo apt-get install cifs-utils
```
**Azure File Share created**
![ScreenShot](screenshots_task4/fileshare-deployed.png)

**Sample objects uploaded to the File Share using Azure Portal**
![ScreenShot](screenshots_task4/files-uploaded-to-fs.png)

**Connection script available in Azure Portal (executed on local Ubuntu 22 machine)**
```bash
sudo mkdir /mnt/practice3fileshare
if [ ! -d "/etc/smbcredentials" ]; then
sudo mkdir /etc/smbcredentials
fi
if [ ! -f "/etc/smbcredentials/practice3task1.cred" ]; then
    sudo bash -c 'echo "username=xxxxxx" >> /etc/smbcredentials/practice3task1.cred'
    sudo bash -c 'echo "password=xxxxxx" >> /etc/smbcredentials/practice3task1.cred'
fi
sudo chmod 600 /etc/smbcredentials/practice3task1.cred

sudo bash -c 'echo "//xxxxx.file.core.windows.net/xxxxx /mnt/practice3fileshare cifs nofail,credentials=/etc/smbcredentials/practice3task1.cred,dir_mode=0777,file_mode=0777,serverino,nosharesock,actimeo=30" >> /etc/fstab'
sudo mount -t cifs //xxxxx.file.core.windows.net/xxxxx /mnt/practice3fileshare -o credentials=/etc/smbcredentials/practice3task1.cred,dir_mode=0777,file_mode=0777,serverino,nosharesock,actimeo=30
```

**New file added from the local Ubuntu machine**
![ScreenShot](screenshots_task4/sample-file-added-locally.png)

**Sample file added locally is also visible in the Azure Portal**
![ScreenShot](screenshots_task4/local-file-in-azure.png)

**Cleaning up**
```bash
sudo umount /mnt/practice3fileshare
sudo rm -rf /mnt/practice3fileshare
df -h | grep practice3fileshare
```
## Task 5
**Azure Storage Table overview**
![ScreenShot](screenshots_task5/table-overview-se.png)

**Populating the table with some data**
![ScreenShot](screenshots_task5/adding-table-data.png)

**Running table queries**
![ScreenShot](screenshots_task5/running-query.png)

**Deleting table entities using specific properties**
![ScreenShot](screenshots_task5/entity-deleted.png)

```bash
az storage entity delete \
  --account-name practice3task1 \
  --account-key ${storage-access-key} \
  --table-name employeedata \
  --partition-key Employees \
  --row-key 1
```

## Task 6
**Creating SAS token for testing blob access (read-only access)**
![ScreenShot](screenshots_task6/sas-blob-read-list.png)

**Connecting to the blob using Azure Storage Explorer and SAS URL**
![ScreenShot](screenshots_task6/connection-via-sas-blob.png)

**Testing blob operations (e.g file upload)**
![ScreenShot](screenshots_task6/upload-not-permitted.png)

**Creating SAS token for testing file share access (read and write access)**
![ScreenShot](screenshots_task6/sas-fileshare-read-write.png)

**Testing file share operations(aploading a file to file share)**
![ScreenShot](screenshots_task6/uploaded-via-sas-fileshare.png)

**Creating SAS token for testing table operations(full access)**
![ScreenShot](screenshots_task6/sas-table-full-access.png)

**Connecting to the table using SAS token**
![ScreenShot](screenshots_task6/table-access-with-sas.png)

**Testing table operations (deleting the table from Storage Explorer directly)**
![ScreenShot](screenshots_task6/table-deleted-with-sas.png)

**Cleaning up the SAS token accesses by rotating signing key (key1)**
![ScreenShot](screenshots_task6/cleaning-up.png)

## Task 7
**Creating an Azure Service Principal with Blob Data Contributor role**
```bash
az ad sp create-for-rbac --name "secure-storage-sp" --role "Storage Blob Data Contributor" --scopes /subscriptions/xxxxx/resourceGroups/xxxxx/providers/Microsoft.Storage/storageAccounts/practice3task1
```
**Logging from Azure CLI terminal using the IDs provided after SP creation**
```bash
az login --service-principal \
  --username xxxxx \
  --password yyyyy \
  --tenant zzzzz
```

**Testing assigned role for Service Principal by uploading a sample file**
```bash
 az storage blob upload \
  --account-name practice3task1 \
  --container-name practice3container \
  --name test.txt \
  --file ~/Desktop/samplefile.txt \
  --auth-mode login
```

**The sample file was successfully uploaded to Azure Storage Account blob container**
![ScreenShot](screenshots_task7/blob-uploaded-with-sp.png)

**Creating another Service Principal with insufficient permissions**
```bash
az ad sp create-for-rbac --name "secure-storage-sp-test" --role "Storage Blob Data Reader" --scopes xxxxx/resourceGroups/xxxxx/providers/Microsoft.Storage/storageAccounts/practice3task1
```

**Testing file upload with our newly created Service Principal (no access to Blob)**
![ScreenShot](screenshots_task7/testing-permissions-for-second-sp.png)

**Azure Virtual Machine created with Managed Identity**
```bash
az vm create \
  --name task7vm-managed-test \
  --resource-group xxxxx \
  --image Ubuntu2204 \
  --admin-username azureuser \
  --admin-password ${adminpwd} \
  --assign-identity "/subscriptions/xxxxx/resourceGroups/MarkiianKhymynets/providers/Microsoft.ManagedIdentity/userAssignedIdentities/task7-managed-identity"
{
  "fqdns": "",
  "id": "/subscriptions/xxxxx/resourceGroups/xxxxx/providers/Microsoft.Compute/virtualMachines/task7vm-managed-test",
  "identity": {
    "systemAssignedIdentity": "",
    "userAssignedIdentities": {
      "/subscriptions/xxxxx/resourceGroups/xxxxx/providers/Microsoft.ManagedIdentity/userAssignedIdentities/task7-managed-identity": {
        "clientId": "xxxxx",
        "principalId": "xxxxx"
      }
    }
  },
  "location": "centralus",
  "macAddress": "xxxxx",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "xxxxx",
  "resourceGroup": "xxxxx",
  "zones": ""
}
```

**Role Assignment with System-Managed Identity for Blob access**
![ScreenShot](screenshots_task7/role-assignment-for-identity.png)

**Accessing the blob from the Azure Virtual Machine CLI**
```bash
az login --identity --username xxxxx
```
```bash
az storage blob list \
  --account-name practice3task1 \
  --container-name practice3container \
  --auth-mode login
```

```bash
[
  {
    "container": "practice3container",
    "content": "",
    "deleted": null,
    "encryptedMetadata": null,
    "encryptionKeySha256": null,
    "encryptionScope": null,
    "hasLegalHold": null,
    "hasVersionsOnly": null,
    "immutabilityPolicy": {
      "expiryTime": null,
      "policyMode": null
    },
    "isAppendBlobSealed": null,
    "isCurrentVersion": null,
    "lastAccessedOn": null,
    "metadata": {},
    "name": "1984-Orwell.epub",
    "objectReplicationDestinationPolicy": null,
    "objectReplicationSourceProperties": [],
    "properties": {
```
## Task 8
**Azure SQL database and SQL server created via an Azure Portal**
![ScreenShot](screenshots_task8/sql%20database%20created.png)

**Creating new Table called Products using SQL Query (SQL authenticated previously)**
```bash
CREATE TABLE Products (
    ID INT PRIMARY KEY IDENTITY(1,1),
    Name NVARCHAR(50) NOT NULL,
    Price DECIMAL(10, 2) NOT NULL
);
```

**Populating previously created table with some data for testing**
```bash
INSERT INTO Products (Name, Price) 
VALUES 
    ('Laptop', 999.99),
    ('Smartphone', 599.99),
    ('Tablet', 299.99);
```

**Verifying that data is in place**
```bash
SELECT * FROM Products;
```
![ScreenShot](screenshots_task8/all-products.png)

## Task 9
**Azure SQL Database created using the Azure Portal**
![ScreenShot](screenshots_task9/db-created.png)

**Azure SQL Server as a prerequisite for SQL database**
![ScreenShot](screenshots_task9/sql-server-overview.png)

**Long Term Retention policy alignment and Overview**
![ScreenShot](screenshots_task9/ltr-policy-created.png)
![ScreenShot](screenshots_task9/ltr-policy-overview.png)

**PITR option available (LTR restore can't be taken or triggered manually)**
![ScreenShot](screenshots_task9/pitr-availble.png)

```bash
Long-term retention (LTR) is not currently compatible with Azure SQL Database when utilizing the serverless configuration with automatic pause
```

## Task 10
**Azure Cosmos DB account overview**
![ScreenShot](screenshots_task10/cosmos-db-account.png)

**Sample data populated into database and container created before**
![ScreenShot](screenshots_task10/data-populated.png)

```bash
{
  "id": "1",
  "name": "Smartphone",
  "category": "Electronics",
  "price": 699
},
{
  "id": "2",
  "name": "Laptop",
  "category": "Electronics",
  "price": 999
},
{
  "id": "3",
  "name": "Shoes",
  "category": "Apparel",
  "price": 79
}
```

**Consistency levels table overview**

| Consistency Level  | Latency       | Availability | Consistency Strength |
| ------------------ | ------------- | ------------ | -------------------- |
|  Strong            |  High         |  Lowest      |  Highest             |
|  Bounded Staleness |  Moderate     |  High        |  High                |
|  Session           |  Low          |  High        |  Medium              |
|  Consistent Prefix |  Low          |  High        |  Medium              |
|  Eventual          |  Lowest       |  Highest     |  Lowest              |


![ScreenShot](screenshots_task10/consistency-overview.png)

**Global replication overview**
![ScreenShot](screenshots_task10/global-replication.png)

**Quering the database before replication**
![ScreenShot](screenshots_task10/query1.png)

**Manual Failover from East US to West Europe (read region to write region)**
![ScreenShot](screenshots_task10/manual-failover.png)
![ScreenShot](screenshots_task10/manual-failover-performed.png)

**Quering the database after manual failover to West Europe**
![ScreenShot](screenshots_task10/query2.png)





















