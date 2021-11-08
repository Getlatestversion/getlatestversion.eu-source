---
draft: true
title: "Untraceable artifacts"
date: 2020-05-23T14:00:00+01:00
authors: 
- giuliov
categories:
- devops
tags:
- build
- top 10 mistakes
summary: 'TODO'
---

TODO

Tracing features from inceptions to delivery in production is crucial. It is important for the business: when software reaches users, it starts to return the value of the investment. It is important, for troubleshooting, to track the exact version of source code in production. Traceability correlates events, which enables measuring your process beyond the single tool in the chain. If you distribute your software, once called shrink wrapped, it is even more important to know exactly the version of each component in the hands of customers.

## No artifact versioning
There are quite many artifacts that have no intrinsic versioning:
•	Scripts: bash, PowerShell, Ansible, etc
•	IaC: ARM Template, CloudFormation(?), Terraform, Pulimi(?)
•	Database schema: 
Javascript?

## Careless versioning
Old Visual Studio default 
AssemblyVersion["1.0.*"]
Never bump the numbers
•	Editing files like AssemblyInfo.cs or pom.xml;
•	Passing parameters like VersionPrefix.

## No linkage between binary and source versions
When you rely on a system like Jenkins or AzP, the source version, typically a Git commit SHA, is recorded along the other build information, so this is an unusual case… except when you generate version numbers for binaries without linking them in any place or letting them hard to discover in build logs.

## No links to issues (work items) or deployments
Premise: I use the expression work items to refer to the records used to plan and track Users Stories, Bug, and others. In GitHub and JIRA they are known as Issues.
This is the only step that requires manual work and discipline.
