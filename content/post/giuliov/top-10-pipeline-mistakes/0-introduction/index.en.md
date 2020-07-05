---
title: "Top 10 Pipeline mistakes"
date: 2020-05-23T14:00:00+01:00
authors: 
- giuliov
categories:
- devops
tags:
- build
- top 10 mistakes
summary: 'Introduction to the Top 10 Pipeline mistakes series'
---

Today I am going to start a series of posts detailing common issues or mistakes in a DevOps context.
I will try to refer to my experience and add some practical suggestion to identify and solve these issues.

Let's start with my list of top 10 CI/CD pipeline issues.

## The list

> **NOTE**:  
> Published posts are in **bold**, the link for items in _italic_  does not work.  
> Last update: 10 June 2020

 1.	[_Sloppy handling of Secrets_](../sloppy-secrets-handling) -- leaking or hard-coding passwords, tokens or similar sensitive data;
 2.	[_Untraceable artifacts_](../untraceable-artifacts) -- when builds produce (or worse: deploy!) binaries of unknown source and version; this is a major red flag because it is cheap and easy to fix, but it is usually overlooked causing a major technical debt pile-up;
 3.	[_Too specific_](../too-specific) -- if your artifacts are not scrubbed from environment-specific dependencies, so they cannot be deployed to all environments;
 4.	[_What, quality?_](../what-quality) -- when your pipeline does not contain any check on quality, what do you expect as a result?;
 5.	[_Bleeding edge_](../bleeding-edge) -- using the latest and greatest technology is not always a wise choice;
 6.	[_Galactic Builds_](../galactic-builds) -- far-reaching builds that slow teams down instead of helping them;
 7.	[**Flaky builds**](../flaky-builds) -- builds generating unreproducible behaviours or random artefacts;
 7.	[**Flaky builds**](../../06/flaky-builds) -- builds generating unreproducible behaviours or random artefacts;
 8.	[_Too much of a good thing_](../too-much-of-a-good-thing) -- when you go too far to avoid the above mistakes, causing the fix to backfire;
 9.	[_Implicit assumption_](../implicit-assumption) -- any build that breaks when some undocumented environmental condition change;
10.	[_Untamed plugins_](../untamed-plugins) -- similar to the previous one, it is the nightmare of people that manage your build environments, when the build software uses too many, or even conflicting plugins.

The list is not really complete: there is one more to add.

## The unforgivable sin: having no pipeline

This is the ultimate sin of any developer: having no automated process of any kind.  
At a minimum you can have a simple bash or PowerShell script to compile and publish your project.
With that it is going to be easy to integrate it in any of the most popular Continuous Integration tools: Jenkins, Azure DevOps, GitHub Actions, GitLab, TeamCity, etc.  
That script should check for dependencies and label the produced artifacts with a version number. This will be discussed in detail in future posts.  
If you do not need to automate the process for other people, at least, automate it for your future self!

See you soon with the next episode.
