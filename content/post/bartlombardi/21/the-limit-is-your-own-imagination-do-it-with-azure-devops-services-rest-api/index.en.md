---
title: "The limit is your own imagination, do it with Azure DevOps Services REST API"
date: 2021-11-03T03:00:00+02:00
authors: ['bartlombardi', Andrea Sturiale]
categories:
- devops articles
tags:
- azure devops rest api
- powershell
summary: "We have taken advantage of the API Rest made available by Azure DevOps through a script in PowerShell to perform release activities."
publishDate: 2021-11-04T03:00:00+02:00
---


Azure DevOps is the platform signed by Microsoft that helps software factories to improve the development process, automatic tests and frequent releases. Their aim is to have additional value for the end customer in terms of product quality.

With Azure DevOps Release you can separate the delivery environments into groups, obtaining integration, acceptance, production environments and so on. Most of time, the pipelines are configured to release the artifacts related to the main branches. However, Azure DevOps doesn't allow to modify easily a specific release for a single target within a Deployment Group starting from different artifacts.

# Automated through the Azure DevOps Services REST API

Consider each developer have VM on the deployment group and needs to test a release with its own changes before merging on the main branch. This is onerous for him unless he's an Azure DevOps user, capable of modifying its deployment group, the branch where to download the artifacts, etc. It's possible to automate all the previous steps needed to launch a release only on your target environment, starting from a particular artifact.

In the following the steps to achive it:
1. set a new tag to the VM in the deployment group
2. put the tag into release pipeline
3. launch the release pipeline
4. remove the tag from deployment group and release pipeline

We have taken advantage of the [Azure DevOps Services REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/?view=azure-devops-rest-6.1) made available by Azure DevOps through a script in PowerShell. 
Here there are the essential steps:

```powershell 
Write-Host "Adding tag for your VM in the Deployment Group: $deploymentGroup"
$UriOrga = "https://dev.azure.com/<organization>/<project>/_apis/distributedtask/deploymentgroups/" + $DeploymentGroupId + "/targets?api-version=6.0-preview.1"
$output = Invoke-RestMethod -Uri $UriOrga -Method get -Headers $AzureDevOpsAuthenicationHeader 
$UriOrga = "https://dev.azure.com/<organization>/<project>/_apis/distributedtask/deploymentgroups/" + $DeploymentGroupId + "/targets?api-version=6.0-preview.1"
Invoke-RestMethod -Method Patch -Uri $UriOrga -Headers $AzureDevOpsAuthenicationHeader -Body $TargetVMs -ContentType application/json

Write-Host "Adding tag to Deployment Group stage in the Realese Pipeline"
$UriOrga = "https://vsrm.dev.azure.com/<organization>/<project>/_apis/release/definitions/" + $ReleaseDefinitionId + "?api-version=6.0"
$UpdateEnvironmentsBody = Invoke-RestMethod -Uri $UriOrga -Method Get -Headers $AzureDevOpsAuthenicationHeader -ContentType application/json 
$UpdateEnvironmentsBody.environments[0].deployPhases[0].deploymentInput.tags = @( $Tag )
$UpdateEnvironmentsBody = ConvertTo-Json $UpdateEnvironmentsBody -Depth 10
$UriOrga = "https://vsrm.dev.azure.com/<organization>/<project>/_apis/release/definitions?api-version=6.0"
Invoke-RestMethod -Uri $UriOrga -Method Put -Headers $AzureDevOpsAuthenicationHeader -Body $UpdateEnvironmentsBody -ContentType application/json 

Write-Host "Launching Release Pipeline for your VM"
$UriOrga = "https://vsrm.dev.azure.com/<organization>/<project>/_apis/release/releases?api-version=6.0"
$CreateReleaseBody = ConvertTo-Json $CreateReleaseBody -Depth 10
Invoke-RestMethod -Uri $UriOrga -Method Post -Headers $AzureDevOpsAuthenicationHeader -Body $CreateReleaseBody -ContentType application/json 

Write-Host "Removing all tags from Deployment Group stage in the Realese Pipeline"
$UriOrga = "https://vsrm.dev.azure.com/<organization>/<project>/_apis/release/definitions/" + $ReleaseDefinitionId + "?api-version=6.0"
$UpdateEnvironmentsBody = Invoke-RestMethod -Uri $UriOrga -Method Get -Headers $AzureDevOpsAuthenicationHeader -ContentType application/json 
$UpdateEnvironmentsBody.environments[0].deployPhases[0].deploymentInput.tags = @()
$UpdateEnvironmentsBody = ConvertTo-Json $UpdateEnvironmentsBody -Depth 10
$UriOrga = "https://vsrm.dev.azure.com/<organization>/<project>/_apis/release/definitions?api-version=6.0"
Invoke-RestMethod -Uri $UriOrga -Method Put -Headers $AzureDevOpsAuthenicationHeader -Body $UpdateEnvironmentsBody -ContentType application/json 
```

# Conclusion

Like any Azure service, Microsoft's DevOps counterpart can also be managed by a set of APIs that replicate everything that can be done through the portal. First you need to get a [PAT (Personal Access Token)](https://docs.microsoft.com/it-it/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=preview-page) through which you can authenticate inside your organization.
Then you can take advantage of any tool that invokes API Rest to do whatever you want, i.e. launching Release, creating or modifying Deployment Groups, starting specific Tests and so on. This can be a hint to create a user-friendly interface for developers not accustomed to DevOps. It could automate set of actions that would be otherwise onerous.

In these cases the limit is your own imagination.
