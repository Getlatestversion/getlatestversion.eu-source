---
draft: true
title: "Too specific"
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

## Environment-specific deploy packages

I found this happening in two scenarios:
-	Web e.g. React Single Page Application
-	Build tied to branch/environment
At the core two problems:
1.	You haven’t tested the new build: side effects, mistakes in the promotion/build process
2.	Lack of flexibility: ops need to change configuration in autonomy (e.g. migrate a resource to a different DNS name)

## The One Exception (and how to manage it properly)

Signing
Better if the same build produces both signed and unsigned artifacts
Next best option is a build that picks the binaries from the artifact store and sign them
