---
draft: true
title: "Too much of a good thing"
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

## Too much versioning
If you increment the version number on each possible build of a repository with 3-4 commits per working day, you end up with a thousand builds each year. Instead you should differentiate internal from published builds: internal releases do not change the version numbers while published releases move the version up.
Follow the SemVer (https://semver.org/) suggestion and avoid using the build number as package version number. Store this information in SemVer “Build metadata”.
Do not store all intermediate versions, learn Maven SNAPSHOT pattern and apply to your context.

## Libraries need different policies from deploy artifacts
How much rollback you need? It depends on a few things:
•	Production vs. lesser environments
•	Years of library support
•	Auditing requirements
