---
draft: true
title: "Galactic Builds"
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

## Do too much and takes too much
I found huge pipelines taking anything from 30 minutes to 5 hours before finishing. In one case the build was compiling all source code of a complex system, every single time, at every check in/commit in version control. No libraries, no artifact store.
In another case, the compile was fast but the testing process immense. Single threaded UI testing. No unit testing, no API level testing, just UI.
## Why it happens?
In many cases, it results from the same forces as technical debt in code. A system outgrows the initial design and become harder to evolve. The real solution is paying back the debt and start refactoring the system. In a similar vein, you add a feature to your pipeline, then another component, than another, and, finally you have a monster.
In other cases, the person who implemented the pipeline interpreted continuous integration to an extreme and pretends to rebuild the whole system from source code every single time, even for development branches. I often term this approach as push, which works very well only if each component of the system can be independently deployed. It does not work well when the system is released as a whole, in scenarios like shrink wrapped or embedded software, and the system size is non trivial. In a similar scenario, it is best to switch to a pull approach, where teams release packaged components, or libraries, with a clear labelling if the released version of a component is for internal use (e.g. SNAPSHOT pattern in Maven). Teams dependent on the package decide whether stick to public versions (and even pin to a specific version) or catch up with the source. There is more to say to the topic of managing dependencies — e.g. how to resolve conflicts in cascade dependencies — but it is outside the scope.
## Slow feedback
Developers need a fast feedback; a team cannot work together and discover the next day of issues in their code.
## Fix: split the process
Have a fast build associated with CI, a slower build associated with PRs. Any slower process, like UI testing, can be run asynchronously or periodically scheduled. Modern testing tools offer smart techniques to minimise the number of tests to run and thus reduce build times. Impact analysis is preeminent.
