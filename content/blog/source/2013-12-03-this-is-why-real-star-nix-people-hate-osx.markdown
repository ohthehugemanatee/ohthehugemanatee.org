---
categories:
- notes
comments: true
date: "2013-12-03T14:11:23Z"
slug: this-is-why-real-star-nix-people-hate-osx
title: This is why real *nix people hate OSX
---
You know what grinds my gears? Unnecessary changes to behaviors of totally standard Linux/Unix commands in OSX. 99% of the time you can use the OSX command line like any sane CLI, but every once in awhile you run into these oddities that are seemingly only there to piss you off. I ran into one today: *sed* behaves differently on OSX. It counts its arguments differently, so you have to explicitly give it an empty argument for -i. So 

`sed -i 's/find/replace/g'`

has to become

`sed -i '' -e 's/find/replace/g'`

There's no benefit here, just Apple being a PITA.
