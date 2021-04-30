---
title: "Serverless is the MS Access of the Future"
date: 2019-01-24T15:12:30+02:00
lastmod: 2019-01-24T15:12:30+02:00
categories : [ "dev", "serverless"]
---

Controversial opinion time: the usefulness of what we presently call "serverless" will always be limited to simple use cases. It is a great choice for glue code or simple projects, but it will never be the best choice for even medium complexity development problems. Containers and similar technologies will eat its lunch the same way RDBMSes ate MSAccess'.

The benefits of serverless are real. Don't worry about infrastructure, don't worry about scaling or availability. Just paste your code here and we'll take care of the rest! Minimal running costs, infinite scalability, simpler units of code to maintain!

These benefits are only possible because the cloud provider made a lot of decisions for you, in a way that is sensible for a majority use case. These are all decisions that you could hypothetically configure for yourself, given the time to do so. And it's exactly here that the core value of serverless is found. If your use case happens to fit within the frame set out by your serverless provider, there's real value on the table in terms of setup and maintenance time/cost (and SLA).

These decisions come with limitations, like most technical choices. What languages and versions can you use? What modules, what dependency management systems? What external binaries are available? What memory or disk is available, with what kind of I/O throughput? What's the local development environment, and how well does it replicate the live one? What are the scaling characteristics? And so on.

In a simple use case, most of these probably don't matter, and the ones that *do* matter are often exposed by the cloud provider. You can choose your VM size, for example, and preemptive scaling rules, and attach disks and external services, and supply secondary binaries yourself, and...

Very quickly you've taken on a similar complexity, setup, and maintenance cost to what you were trying to avoid in the first place. This might be OK if it were a net zero transaction, but it's not. You've taken on those costs, in exchange for... the rest of the limitations and a platform over which you have no control.

This feels a lot like so many MSAccess applications I worked with in the early 2000's. When the application was simple, it was great to have a visual data engine. But with a larger data model, the UI increasingly became an obstacle. You would increasingly use text to express your queries, duplicate data in more convenient tables to avoid tricky joins, and ... In the end, the workarounds piled up. Managing a complex application on MSAccess is just as hard as managing it on any other RDBMS, but without the flexibility and power, and with more kludgy workarounds. Access is a great product for simple or straightforward use cases. But the moment your application grows too big, you start paying a heavy tax.

It's not controversial to suggest that the core value of Serverless is providing OOTB great hosting for simpler, highly encapsulated code, even code segments. The controversial part is the future prediction, where the metaphor really kicks in.

What happened to MSAccess? Other RDBMS' got much better, and friendlier. From easy-to-use ORMs for easy-to-use programming languages, to GUIs like MySQLAdmin, to cloud-based application builders backed by RDBMSes, the full-powered RDBMS ecosystem gradually took over the use case for MSAccess. The ease-of-use benefit which was so core to the Access value proposition gradually disappeared, and users ended up with the choice between a fully-flexible power system and a limited one, both relatively easy to use. Finally Access 2010 became a GUI on top of SQL, integrated with Sharepoint.

Serverless is headed in a similar direction. Other tools, largely from the container ecosystem, are already nibbling at their lunch. If you're at the point where you need to configure VM sizes and scaling rules on your Serverless provider, you're probably considering jumping to a managed Kubernetes provider instead. If you don't need to configure that stuff, you're looking at pure-container cloud solutions like Azure Container Instances. Same flexibility and cost structure, but with run-anywhere compatibility and a development environment which matches the CI testbed and prod. The only uncontested ground left is applications that are too small to bother containerizing.

Meanwhile the container ecosystem is taking off at rocket speed, making that "flexibility tradeoff" worse and worsse for serverless. Where is the multi-cloud ecosystem for serverless? Where is the network security modeling market? Compare the difficulty of local dev and hosted CI environments for serverless code, to the out of the box auto-detected container builds available in every major CI platform. I's clear: serverless isn't actually all that much easier anymore. And it's only going one way. Soon enough, Serverless will become a nice frontend for a container runtime.
