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


