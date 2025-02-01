# Azure DevOps Course tasks results (Practice #4)

## Task 1

**Sample Flask application written in Python**
![ScreenShot](screenshots_task1/sample-python-app.png)

**Application dependencies described in requirements.txt**
![ScreenShot](screenshots_task1/application-dependencies.png)

**Sample Dockerfile for application dockerizing**
![ScreenShot](screenshots_task1/dockerfile-written.png)

**Azure Container Registry created with Admin access enabled (for ACI usage)**
![ScreenShot](screenshots_task1/container-registry-created.png)

**Building docker image locally in the Ubuntu CLI**
```bash
docker build -t practice4task1.azurecr.io/flask-aci-app:v1 .
```

**Fixing local issue with DNS resolution (thanks to StackOverFlow)**
```bash
 Step 4/7 : RUN pip install -r requirements.txt
 ---> Running in 7f4635a7510a
Collecting Flask (from -r requirements.txt (line 1))

Retrying (Retry(total=4, connect=None, read=None, redirect=None)) after 
connection broken by
'NewConnectionError('<pip._vendor.requests.packages.urllib3.connection.VerifiedHTTPSConnection 
object at 0x7fe3984d9b10>: Failed to establish a new connection: 
[Errno -3] Temporary failure in name resolution',)': /simple/flask/

```
```bash
#FIX:
$ sudo vi /etc/docker/daemon.json
{
    "dns": ["8.8.8.8", "8.8.4.4"]
}
$ sudo service docker restart
```

**Login to ACR and pushing the docker image**
```bash
az acr login --name practice4task1
docker push practice4task1.azurecr.io/flask-aci-app:v1
```

**Azure Container Instance created and linked to ACR**
![ScreenShot](screenshots_task1/aci-overview.png)

**Sample Flask application deployed using ACI+ACR**
![ScreenShot](screenshots_task1/app-redeployed.png)

## Task 2
**Modified sample code for Flask application written in Python**
![ScreenShot](screenshots_task2/sample-app-flask.png)

**Logging into ACR, building and pushing new docker image**
```bash
 $ az acr login --name practice4task1
 $ docker build -t practice4task1.azurecr.io/flask-aci-app:v2 .
 $ docker push practice4task1.azurecr.io/flask-aci-app:v2
```
**New ACI container deployed with MESSAGE environment variable specified**
![ScreenShot](screenshots_task2/aci-deployed.png)

**Browsing <public IP>:5000 for application demo**
![ScreenShot](screenshots_task2/application-deployed.png)

## Task 3












