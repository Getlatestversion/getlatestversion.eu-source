---
title: "Git Cherry-Pick for a strategic harvest"
date: 2020-07-13T03:00:00+02:00
authors: ['bartlombardi']
categories:
- devops articles
tags:
- azure devops
- git
- cherry pick
summary: 'Fixing a bug on multiple branches is a complex task. In this article I will illustrate the Git cherry-pick command.'
publishDate: 2020-07-13T03:00:00+02:00
---

# Introduction

The IT companies that develop and sell software product are always focused on delivering new features to propose new versions of them to the final client, still considering the maintenance of the previous release.
Generally, we are talking about projects of millions of lines of code that requires multiple developing teams located around the world with strong coordination and collaboration among them, in which it is necessary to handle multiple versions of the same product with a significant number of release branches. Fixing a bug on multiple branches is a complex task because the developer has to report the same corrections in the various release branches. This action is possible in Git and it is called "cherry-picking".

# Git cherry-pick

One of the most powerful commands available in Git is Cherry-pick. It takes as inputs one or more commits and applies them to a different branch, by creating a new commit. 

Cherry-pick is commonly used in many Git workflows such as the Azure DevOps team’s, described in the following [article](https://devblogs.microsoft.com/devops/improving-azure-devops-cherry-picking/), see the image reported below for a representation of the workflow.
![Cherry-Pick: way of working](cherry-pick-workflow.jpg)

Anytime a developer works with many version of the same product it is essential to assure that all the bug reported have been fixed in every version. The cherry-picking action is performed into the main branch also to avoid deploying a new release with the same bug.

## Azure DevOps Repos

Azure DevOps provides the cherry-pick of a completed Pull Request (PR) or of a single commit by clicking a dedicated button. The process will create a new PR with the same modification on the branch in case a fix is needed. 
![Cherry-Pick Azure DevOps](azdo-cp.jpg)

In advance, a [PR Multi-Cherry-Pick](https://github.com/microsoft/azure-repos-pr-multi-cherry-pick) is possible by means of an open source extension available on Azure DevOps Marketplace.

In case Azure Devops warns out that cherry-pick cannot be performed automatically, see the picture below, this is related to conflicts generated during the merge, therefore it has to be performed locally. 
![Azure DevOps conflict errors](azdo-cp-error.jpg)

Many development environments integrated within Git carry out this local operation by graphic user interface. Best candidates are Visual Studio or VS Code.

# Git cherry-pick --continue

Few steps are needed to execute the following command [git cherry-pick](https://git-scm.com/docs/git-cherry-pick). 

A first action consist in taking note of hash commit that you want to cherry-pick. In the specif case of a PR the list of commit is available in the **Commits** tab of Azure DevOps Repos, see the following images.
![Commit table on Azure DevOps](azdo-commits-tab.jpg)

Once you are on the branch (```git checkout <branch-name>```) that you want to commit, the following command must be executed ```git cherry-pick <commit>```.

Knowing that the cherry-pick shown in the previous example generates a conflicts, once you have done the marge manually with Visual Studio (or other IDE), the ```git cherry-pick --continue``` command must be executed to proceed. If you want to abort the process simply digit ```git cherry-pick --abort```.

Anytime the process is completed it is necessary to perform ```git push```of the new generated commit.
