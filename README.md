# Azure DevOps Course tasks results (Practice #1)

## Task 1

**Custom tenant created after Entra ID P2 trial activated**
![ScreenShot](screenshots_task1/tenant.png)

**Users created**
![ScreenShot](screenshots_task1/users.png)

**Admins and Devs Groups**
![ScreenShot](screenshots_task1/groups.png)

**Dev group role assignment**
![ScreenShot](screenshots_task1/devs_group.png)

**User added to the relevant group**
![ScreenShot](screenshots_task1/user1.png)

**Permissions inheritance example on second user**
![ScreenShot](screenshots_task1/permissions_inherited.png)

## Task 2 (skipped)

## Task 3
**Resource Viewer custom role JSON file**
![ScreenShot](screenshots_task3/resource_viewer.png)

**Role definition created**
![ScreenShot](screenshots_task3/role_definition.png)

**Roles assignment sample command**
![ScreenShot](screenshots_task3/role_assignment.png)

**Admins group role verification**
![ScreenShot](screenshots_task3/admins_verification.png)

**Developers group role verification (no permissions to use View my access, but Resource group is visible, so assignment is successful)**
![ScreenShot](screenshots_task3/developers_verification.png)

## Task 4
**Azure Key Vault created**
![ScreenShot](screenshots_task4/key-vault-general.png)

**No list access for user Marcus**
![ScreenShot](screenshots_task4/no-list-access.png)

**Access policy applied for Developers group**
![ScreenShot](screenshots_task4/access-policy.png)

**Antonio from Devs group logged in via CLI**
![ScreenShot](screenshots_task4/antonio-login.png)

**Secret retrieval using CLI (created earlier)**
![ScreenShot](screenshots_task4/secret-retrieved-cli.png)

## Task 5
**Key Vault deployment**
![ScreenShot](screenshots_task5/key-vault-deployment.png)

**Policy definition**
![ScreenShot](screenshots_task5/policy-definition.png)

**Policy assignment**
![ScreenShot](screenshots_task5/policy-assignment.png)

**Policy state evalueation for specific Resource Group**
```bash
$ az policy state trigger-scan --resource-group "azure-devops-rg"
```

**Non-compliant policy**
![ScreenShot](screenshots_task5/key-vault-non-compliant.png)

**Non-compliant resource detailed view**
![ScreenShot](screenshots_task5/non-compliance-detailed.png)

**Optional: Deny policy in action**
![ScreenShot](screenshots_task5/deny-policy-in-action.png)

















