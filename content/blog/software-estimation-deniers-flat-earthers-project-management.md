---
title: "Software Estimation Deniers: the Flat-Earthers of Project Management"
date: 2022-12-20T09:01:00+01:00
lastmod: 2022-12-20T09:01:00+01:00
categories : [ "dev", "project management", "estimation" ]
highlight: false
---
Another day, another [Hacker News post](https://news.ycombinator.com/item?id=34061206) complaining about estimation in software build projects. These articles and comment sections should come with a trigger warning for armchair speculation. **Project management and time estimation are fields of serious research. Drawing conclusions from your personal experience, with no idea what the research says, is as silly as writing your experience that the world is obviously flat.**

Estimation is actually well understood in academic project management. There is (Nobel Prize winning!) research about what, actually, are the problems inherent to estimation, and how to produce specific and accurate estimates despite them. This academic field is almost 50 years old, and no one who complains about estimation in blog posts or comments is aware of it.

Stop navel gazing and actually go READ about the subject. I know it's hard for us to take in anything longer than a StackExchange post, but please try, BEFORE you write about your shitty experience with estimates and generalize to the entire problem space.

Here are some things that the research has said for *decades*:

- Humans are *all* bad at time estimation. Even the ones who consider themselves good at it, estimating tasks with which they are very familiar, "only" underestimate by 30% *at best*.

- Humans are pretty good at estimating non-time attributes of work, even those with a direct correlation to time. Like effort, complexity, or "cups of coffee."

- if you estimate something with a time corellation (e.g. complexity) in a consistent way and measure the average throughput over time, you can very precisely and accurately estimate time to completion. This is the Law of Large Numbers, which is how casinos can accurately predict profits when dealing with much more randomness than exists in software projects.

- Note that long term estimates produced with the Law of Large Numbers include unexpected complexity, personal issues, illness, windows updates, etc. It's a statistical law.

- the accuracy of average time estimates is proportional to the time left on the project. It runs opposite to the uncertainty of distant features. I.e. this method does not predict how much you can build in a week; you're better off with your relatively intimate knowledge of the feature at that point in time and a gut check. Rather it predicts how much you will build over 12 weeks, with extraordinary accuracy.

- estimates are better understood when presented with a confidence interval, e.g. "The work as we understand it today will take 8 weeks, with 95% certainty."

- Estimates are damaging to teams when they are treated as proscriptive. They are constructive when used as predictions. i.e. rather than "you must get this work done in 8 weeks," it's "we understand this as 8 weeks of work." Proscriptive estimates compound the problems with human estimation (see first point).

What I HAVEN'T seen in the research, but which is undoubtedly true, is that most teams violate these fundamentals. A certain percentage of those team members then complain on the Internet that estimates are useless.

Asking your team to estimate in time units IS useless. Summing those time estimates to create a long term plan is doubly useless. Cracking the whip on your team when that estimate proves incorrect is triply useless. And complaining about it on the Internet because you've never read any of the grown up work on the subject... well that's hacker culture.

