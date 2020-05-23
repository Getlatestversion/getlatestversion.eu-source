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

Today I start a series of posts detailing recurrent mistakes.
I will try to refer to my experience and add some practical suggestion to identify and solve the issue.

Let's start with my list of top 10 Pipeline Mistakes.

## The list

**NOTE**: the links will work only when the matching post is published.

 1.	[Sloppy handling of Secrets](../sloppy-secrets-handling) -- leaking or hard-coding passwords, tokens or similar sensitive data;
 2.	[Untraceable artifacts](../untraceable-artifacts) -- when builds produce (worse: deploy!) binaries of unknown source and version; this is a major sin because it is cheap and easy to fix;
 3.	[Too specific](../too-specific) -- if your artifacts cannot be deployed to all environments, but each environment receives different artifacts;
 4.	[What, quality?](../what-quality) -- when your pipeline impose no checks on quality, what do you expect as a result?
 5.	[Bleeding edge](../bleeding-edge) -- using the latest and greatest technology is not always a wise choice;
 6.	[Galactic Builds](../galactic-builds) -- uber-encompassing builds that slow down teams instead of helping;
 7.	[Flaky builds](../flaky-builds) -- builds generating unreproducible artifacts or exhibiting random-behavior;
 8.	[Too much of a good thing](../too-much-of-a-good-thing) -- when you go too far to avoid the above mistakes;
 9.	[Implicit assumption](../implicit-assumption) -- any build that breaks when some unstated environmental condition change;
10.	[Untamed plugins](../untamed-plugins) -- similar to the previous, it is the nightmare of people that manage your build environment, when the build software uses too many, even conflicting plugins.

The list is not really complete: there is more.

## The unpardonable sin: no pipeline

This is the main sin of coders: no automated process of any kind.
At a minimum you can have simple bash or PowerShell script to compile and publish your project.
With that it is easy to run it automatically from any of the most popular Continuous Integration tools: Jenkins, Azure DevOps, GitHub Actions, GitLab, TeamCity, etc.
That script should check for dependencies and stamp the artifacts with a version number as we will discuss in detail in future posts.
If you do not need to automate the process for other people, at least, automate for your future self.

See you soon with the next episode.