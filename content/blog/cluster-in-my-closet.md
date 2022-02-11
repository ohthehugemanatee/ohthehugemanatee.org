---
title: "The Cluster in My Closet - Advice for running kubernetes at home"
date: 2021-05-15T15:00:05+01:00
lastmod: 2021-05-15T15:00:05+01:00
categories : [ "dev", "kubernetes", "homelab" ]
---

What do most people do with their old computers? I've never been good at getting rid of mine. They were all repurposed into servers, running whatever key services for my household I could think of at the time. This year I decided to move out of the "old sysadmin" patterns of my roots, and try running my homelab in a more modern way. That's right: I set up a kubernetes cluster.

All my old devices now go to the great cluster in the sky, and live on serving us files and media, blocking ads, and running small automations. They're happy, now.

They may be happy, but I am not. I thought I was comfortable with Kubernetes, but boy was I wrong. Turns out, I was comfortable with kubernetes... in a stable, homoegeneous, cloud-based environment. Turns out, those cloud vendors really do cover a lot of complexity for you: using Azure Kubernetes Service to manage enterprise and microservices apps is *way easier* than running your own cluster for the kind of apps you use in the home.

My home cluster consists of four Raspberry pi 4s, two old laptops, one mini desktop, and a pine64 laptop. First pain point: that's a mix of CPU architectures and capabilities. I've got it all covered: x64, arm7, and arm64... and many container images are only built for a subset of those. At least the laptops are x64, but they're different makes and models, with different quirks. One of them has an unreliable USB port, which wreaks intermittent havoc with its USB ethernet adapter. The other has a dead internal IDE chip, and has to boot off of an external drive duct-taped to the back of the screen. And of course the whole thing lives in my laundry room, where temperatures and vibration from the floor will vary with the drying cycle, and where dust is all-pervasive.

I'm happy I've stuck with it - this has made me *really* learn the ins and outs of detailed troubleshooting in kubernetes. I've a much better understanding now of *why* different abstractions may exist, in a very practical way. Here are some of the fun features and workarounds I have in place. Each of these could be their own post, someday:

- Lots of home-oriented services run with SQLite as a backing database, which is a problem because sqlite's normal 'wal mode' of accessing database files has a hard dependency on block-level file access. That means network filesystems, and all k8s' built in abstractons -  are out.
  - I ran with iSCSI devices for awhile, but the unreliability of home networking hardware was sufficient to produce data corruption every few months.
  - I wrote my own sidecar service to periodically freeze a shared PV and sync all the files to NFS. But if the freeze comes at a bad time for sqlite, that will also cause data corruption.
  - Longhorn doesn't have images for armv7 yet.
  - Finally I settled on using the local provisioner for volumes, and using [litestream](https://github.com/benbjohnson/litestream) to back the DBs up to NFS. This seems to be working well... even though the Pi SD cards are a slow place to write even interim data. Not to mention, when you use the local provisioner, your pod is always scheduled back to the same node. That breaks the flexibility which is half of the value of the system in the first place!
- Very few container images offer all three of my CPU architectures, so every one of my manifests needs to use `NodeSelector`.
- I'm running a single master k3s node on one of the Raspberry Pi 4s. That's plenty of capacity for a normal load, but when you add monitoring through [Netdata](https://www.netdata.cloud/) and prometheus, log monitoring through Loki and Grafana, and a handful of multi-master applications, the load from cluster DNS can cause problems. I ended up implementing [Nodelocal DNS cache](https://kubernetes.io/docs/tasks/administer-cluster/nodelocaldns/) to lighten the load.
- The tiny fans that come on most Raspberry Pi cases are not reliable in the difficult environment of my laundry room. I've had to replace all the case fans several times and eventually bought a rack for the Pis.
- One of my most critical services is Plex, which does much better with hardware decoding capabilities. I'm using [node-feature-discovery](https://github.com/kubernetes-sigs/node-feature-discovery) to get node capabilities into labels, and [intel-gpu-plugin](https://github.com/intel/intel-device-plugins-for-kubernetes/blob/main/cmd/gpu_plugin/README.md) to make the hardware available in the pod. Oh, but node feature discovery doesn't have a build for armv7, so that only works on some nodes.
- I use images from [linuxserver.io](https://linuxserver.io/), which are great... but all based on [s6_overlay](https://github.com/just-containers/s6-overlay). This means they're not compatible with common privilege restriction approaches on kubernetes like security contexts. Fortunately(?) they support including arbitrary scripts on container startup, so I can hack my way around most problems.
- Internal services often prefer to use HTTPS. Fair enough, but they are internal services, so letsencrypt can't validate/issue certificates for them. That means either an internal CA, or app configurations that allow self signed certs.
- I mentioned I have one node with an unreliable ethernet connection. I tuned kubernetes' heartbeat and status check timings to minimize downtime when that node disappears, detecting it early and redistributing its' pods. But it's a delicate balance: it's easy to get the math wrong, or just be overzealous, and your nodes start popping into `NotReady` state with no discernable reason. Of course that disrupts intra-service communication, which can cause cascading failures.
- One of my cats knocked a couple of nodes down while I was away on vacation without physical access to the machines. I ended up building a private network connection to Azure and adding support for scaling with VMs there, to keep services running.

It goes on and on. All of these are problems that you basically never encounter when running on a cloud provider. Your money really does go to a valuable purpose.

On the other hand, my graveyard of machines has taught me more about internals of enterprise orchestration than I ever would have learned by implementing in real-world enterprise contexts. So it's been worth it for me professionally, at least.

## What's the point?

The moral of the story is: running a home kubernetes cluster is a lot harder than you think; and if you have half a brain you already think it's pretty hard. It definitely does have benefits (beyond learning) though! My home services *are* actually self-healing, easy to diagnose, and very resiliant to failure. Capacity planning is a non-issue. I have a repo of human-readable files which describe my application environments in their entirety. The trade off has been worth it for me, but only when I look through a pretty broad lens. I could have spent less time on this had I just stuck with docker-compose, for example.

If you're considering using kubernetes for your home lab, here's my hard-won advice for you:

- Use uniform nodes. Identical CPUs and capabilities make your life so much better.
- Use a lightweight kubernetes distro, like [k3s](https://k3s.io/). It's all API compatible anyway and the community will be full of users in similar situations to yours.
- Don't worry about always doing things the Kubernetes Way. For many home applications, StatefulSets really do make for a more reliable result.
- Set up centralized logging first. The [Loki stack](https://github.com/grafana/helm-charts/tree/main/charts/loki-stack) helm chart is relatively turnkey.
- Then set up centralized monitoring. [Netdata](https://netdata.cloud/) beats the hell out of manually configuring everything in grafana.
- Keep your manifests in a repo, for the love of all that is good in the world.
- If any of your services are mission critical for your family members (Nexcloud and Plex in my house), leave them out of the cluster for as long as possible. Architect those applications as High-Availability. In my case, that means a multi-master mariadb cluster, multiple web heads, and layered failover... with all the health checks I can think of.
- Automate node setup with rancher, ansible, or similar. It's hard to make home hardware act like "cattle instead of pets"; use every tool at your disposal.
- Keep a timestamped log of every change you make that isn't in a YAML file, and every problem you encounter. Often that's the fastest route to find your foot guns.
- Have fun! Remember you're (hopefully) not doing this to be pragmatic. Don't listen to the haters, as long as *you're* getting what *you* want out of the experience.
