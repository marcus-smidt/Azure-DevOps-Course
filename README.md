# Azure DevOps Course tasks results (Practice #7)

## Task 1
**Ubuntu 22.04 VM was created to be added a self-hosted agent**
![Screenshot](screenshots_task1/3vm.png)


**Agent configured**
```bash
$ mkdir myagent && cd myagent
$ wget -O ~/myagent/vsts-agent.tar.gz https://vstsagentpackage.azureedge.net/agent/4.251.0/vsts-agent-linux-x64-4.251.0.tar.gz
$ tar zxvf ~/myagent/vsts-agent.tar.gz
$ ./config.sh
      - url org : https://dev.azure.com/AzureDevOpsCoursexxx
      -PAT : ******
      -Agent Pool: MyLinux
      -Agent Name: myAgent
      - skip 
$ /run.sh
```
![Screenshot](screenshots_task1/1agent-added.png)

**Note: Repository created by Dmytro, ssh key authorization added (ssh-rsa)**


## Task 2
**Git commands used in the task**
```bash
$ git checkout -b feature-enhancement
$ touch FEATURES.md

# Potential Features
## Feature 1
**GitOps approach implementation**
## Feature 2
**Automatic clean up on Friday**
$ sudo rm -rf *
## Feature 3
**Lorem ipsum**

$ git add .
$ git commit -m "Ticket#12345 Features planning"
$ git push --set-upstream origin feature-enhancement
```

**PR created, reviewer added**
![Screenshot](screenshots_task2/1PRcreated.png)
![Screenshot](screenshots_task2/2pr-completed.png)

## Task 3














