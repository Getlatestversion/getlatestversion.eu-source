---
draft: true
title: "Untamed plugins"
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

This is about controlling the build environment, that is the ecosystem that transform and transport source code in the hand of the user.
Most build tools have an extensibility mechanism, called plugins or extensions, that enhance the capabilities or language of the tool. This is good but there is a downside: your build depends as much, if not more, on plugins than the core tool itself; Jenkins is most plagued by this issue.
One issue is future build reproducibility: which plugin versions you used at a specific point in time?
Another issue is keeping the system in a sane state: new plugin versions not compatible with core tool, new version of the core breaks existing plugins, plugins security and quality.
You can see that more recent CI tools explore new approaches to avoid coupling and allowing easy versioning and backup. Particularly interesting is GitHub Actions, you can embed required plugins in your own repository or submodules.
## Old plugins blocking upgrades
## Security issues
## Support issues
## Tedious setup of new build infrastructure
(technical debt)
