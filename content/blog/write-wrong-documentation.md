---
title: "Write Wrong Documentation"
date: 2022-05-02T11:05:09+02:00
lastmod: 2022-05-02T11:05:09+02:00
tags : [ "dev", "documentation", "open source"]
draft: true
---
It's hard to get engineers to write good documentation. There, I said it.

It's hard for a few reasons:

- Good documentation is clear, well structured, and communicative. All traits which are not common or easy for many engineers.
- Good documentation offers technical depth to those who want/need it. That understanding is not available to non-technical writers like marketers or content specialists.
- Good documentation tracks changes in the code closely. Only engineers can follow that as precisely as it needs to be followed.
- Documentation takes time to write, and for an engineer, often feels less productive than building new functionality. It is certainly less satisfying.

I learned a great trick for overcoming this challenge from [Rob Douglass](https://twitter.com/robertDouglass): first write wrong documentation. Before every release, ask marketing or content people to write great documentation that clarly explains how they *think* the product works on the inside. Then ask the engineers to check it, because "I'm pretty sure X doesn't work that way."

The first draft will be wrong on most technical details, and it will drive your engineers *crazy* once they know about it. But the corrections will mostly be localized in nature, leaving the overall tone and structure (from marketing/communications folks who specialize in this stuff) intact.

The result is a best of both worlds: documentation that is at once well structured and technically accurate/complete, with a regular update cycle.
