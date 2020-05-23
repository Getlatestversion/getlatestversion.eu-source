---
draft: true
title: "Flaky builds"
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

These builds have some kind of unpredictable outcome. This is bad because you expect your CI to be algorithmic, no randomness there. Worse, you will need to run the same build in a future time, maybe in the harry of delivering a patch, and it fails unexpectedly. Murphy’s Law never ceases.

## Same source, different binaries

Have you tried comparing binaries built on two different machines? 99% of times they will be different! Even if you use the exact same source code version.

I am in the camp for repeatable builds, that is the same source generates the same artifacts. The only acceptable exception is a build log / build id that shows in release notes or similar text files.
Some compilers have a different opinion. Traditionally Microsoft tools embedded some kind of build timestamp in binaries. In more recent time this behaviour can be disabled.
https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/compiler-options/deterministic-compiler-option
https://maven.apache.org/guides/mini/guide-reproducible-builds.html
https://zlika.github.io/reproducible-build-maven-plugin/
https://reproducible-builds.org/
https://blog.conan.io/2019/09/02/Deterministic-builds-with-C-C++.html

## Tests randomly pass/fail
A beautiful essay on flaky tests https://martinfowler.com/articles/nonDeterminism.html
CI tools help manage the scenario by automatically rerunning failed tests and report which tests pass on a second run.
https://docs.microsoft.com/en-us/azure/devops/pipelines/test/flaky-test-management?view=azure-devops
https://docs.gitlab.com/ee/development/testing_guide/flaky_tests.html
https://plugins.jenkins.io/flaky-test-handler/

## Loose dependencies specifications
Four reasons:
•	Loose specification & newer packages versions available
•	Newer versions available for indirect dependencies
•	Loss of package source (direct or indirect)
•	Change in package manager and its algorithm
Lock file mechanism to fix package version either direct or indirect.
https://github.com/NuGet/Home/wiki/Repeatable-build-using-lock-file-implementation
https://docs.npmjs.com/configuring-npm/package-locks.html
https://docs.gradle.org/current/userguide/dependency_locking.html
http://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html#Dependency_Management

