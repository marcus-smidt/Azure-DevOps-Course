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
**Minimal number of reviewer added to ADO Repos main branch**
![Screenshot](screenshots_task3/1policy-added.png)

**Mandatory reviewer (admin) added to the main branch**
![Screenshot](screenshots_task3/2reviewer-policy.png)

**Build policy in place as well (validating code by pre-merging and building pull request changes)**
![Screenshot](screenshots_task3/3build-policy-added.png)

**Branch policies in action (PR initiated and actions automatically performed)**
![Screenshot](screenshots_task3/4pr-initiated.png)

## Task 4
**New repo MySampleApp was created; showing code sample**
![Screenshot](screenshots_task4/1repo-created.png)
![Screenshot](screenshots_task4/sample-node-js.png)

**Azure DevOps Pipeline YAML code**
```bash
trigger:
- main

pool:
  name: MyLinux
  demands:
    - agent.name -equals myAgent

steps:
- task: NodeTool@0
  inputs:
    versionSpec: '18.x'
  displayName: 'Install Node.js'

- script: |
    npm install
  displayName: 'Install dependencies'

- script: |
    npm run build
  displayName: 'Build application'

- script: |
    npm test
  displayName: 'Run tests'

# Use a different approach for creating the artifact
- script: |
    npm install archiver --no-save
    node -e "
    const fs = require('fs');
    const path = require('path');
    const archiver = require('archiver');
    
    // Create a file to stream archive data to
    const output = fs.createWriteStream('$(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip');
    const archive = archiver('zip', { zlib: { level: 9 } });
    
    output.on('close', () => {
      console.log('Archive created successfully');
    });
    
    archive.on('error', (err) => {
      throw err;
    });
    
    archive.pipe(output);
    
    // Add the files
    archive.directory('dist/', false);
    archive.file('package.json', { name: 'package.json' });
    
    archive.finalize();
    "
  displayName: 'Create archive'
    
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(Build.ArtifactStagingDirectory)'
    ArtifactName: 'drop'
    publishLocation: 'Container'
```

**Running the pipeline and reviewing the bild details**
![Screenshot](screenshots_task4/pipeline-successful-run.png)
![Screenshot](screenshots_task4/pipeline-successful-1.png)

## Task 5















