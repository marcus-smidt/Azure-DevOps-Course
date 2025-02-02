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
**Bash script for deploying sample Azure Container App service**
```bash
#!/bin/bash

set -e

echo "Adding Azure Container Apps extension to Azure CLI"
# az extension add -n containerapp --upgrade

echo "Ensuring Microsoft.Web and Microsoft.App providers are registered with current Azure Subscription"
# az provider register --namespace Microsoft.Web
# az provider register --namespace Microsoft.App


rgName=MarkiianKhymynets
location=eastus

acaEnvironmentName=env-practice4task3
lawName=practice4task3
image=thorstenhans/gopher:hero

echo "Creating Log Analytics Workspace"

lawClientId=$(az monitor log-analytics workspace create --workspace-name $lawName -g $rgName --query customerId -o tsv)
lawClientSecret=$(az monitor log-analytics workspace get-shared-keys -n $lawName -g $rgName --query primarySharedKey -o tsv)

echo "Creating Azure Container App Environment"

az containerapp env create -n $acaEnvironmentName -g $rgName --logs-workspace-id $lawClientId --logs-workspace-key $lawClientSecret -l $location
echo ""

echo "Deploying Container to Azure Container App"

fqdn=$(az containerapp create -n hello-aca -g $rgName --environment $acaEnvironmentName --image $image --target-port 8080 --ingress external --query configuration.ingress.fqdn -o tsv)
echo ""

echo -e "Our Azure Container App is up and running at https://${fqdn}"
```

**Azure Container App successfully deployed**
![ScreenShot](screenshots_task3/container-app-deployed.png)

**Browsing sample application URL**
![ScreenShot](screenshots_task3/browsing-the-app.png)

**Scaling to 3 replicas using new revision/deployment**
![ScreenShot](screenshots_task3/scaling-to-3.png)

**Replicas detailed overview on the Azure Portal**
![ScreenShot](screenshots_task3/replicas.png)

## Task 4
**Azure Container Instance created with hello-world container**
![ScreenShot](screenshots_task4/aci-created.png)

**Browsing the Public IP address of the container**
![ScreenShot](screenshots_task4/container-test.png)

**Enabling system-managed identity for ACI service**
![ScreenShot](screenshots_task4/managed-identity-on.png)

**Assigning our Identity with Key Vault Reader role (onle metadata view)**
![ScreenShot](screenshots_task4/role-assign-reader.png)

**Testing on reading the metadata and the actual Secret created before**
![ScreenShot](screenshots_task4/reader-test.png)
![ScreenShot](screenshots_task4/key-vault-reader.png)

**Assigning the needed role for secret retrieval e.g Secret User**
![ScreenShot](screenshots_task4/role-assign-user.png)

**Testing Secret retrieval from our ACI container environment**
![ScreenShot](screenshots_task4/secret-user.png)

## Task 5
**Azure Kubernetes Service cluster created (dev/test, D-series node pool 1-3)**
![ScreenShot](screenshots_task5/cluster-created.png)

**Logging into cluster environment using Azure CloudShell and interacting with cluster**
```bash
Requesting a Cloud Shell.Succeeded. 
Connecting terminal...

Welcome to Azure Cloud Shell

Type "az" to use Azure CLI
Type "help" to learn about Cloud Shell

Your Cloud Shell session will be ephemeral so no files or system changes will persist beyond your current session.
x [ ~ ]$ az aks get-credentials --resource-group xxx --name practice4task5

Merged "practice4task5" as current context in /home/x/.kube/config

x [ ~ ]$ kubectl get nodes

NAME                                STATUS   ROLES    AGE    VERSION
aks-agentpool-39279877-vmss000000   Ready    <none>   7m6s   v1.30.7

x [ ~ ]$ kubectl get pods
No resources found in default namespace.
x [ ~ ]$ kubectl get pods -n=system
No resources found in system namespace.

x [ ~ ]$ kubectl create deployment nginx --image=nginx
deployment.apps/nginx created

x [ ~ ]$ kubectl expose deployment nginx --port=80 --type=LoadBalancer
service/nginx exposed

x [ ~ ]$ kubectl get pods
NAME                    READY   STATUS    RESTARTS   AGE
nginx-bf5d5cf98-299lh   1/1     Running   0          22s

x [ ~ ]$ kubectl get svc
NAME         TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE
kubernetes   ClusterIP      10.0.0.1       <none>          443/TCP        9m15s
nginx        LoadBalancer   10.0.117.112   64.236.77.138   80:31366/TCP   16s
```
**Nginx default page overview browsing the EXTERNAL-IP provided above**
![ScreenShot](screenshots_task5/nginx-default.png)

## Task 6




























