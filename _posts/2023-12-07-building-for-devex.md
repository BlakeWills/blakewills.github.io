---
layout: single
title:  "Building for DevEx"
date:   2023-12-07 20:30:00 +0000
tags: devex
classes: wide
header:
  og_image: /assets/img/posts/2023-12-01-azure-deployment-environments/DeployedEnvironment.png
---

I've been thinking about Developer Experience a lot recently, and something that sticks out is the overlap with SRE. As an SRE I'd work across the engineering organisation, and to get buy-in from the engineers I'd have to show value. One of the easiest ways to do that was by making their lives easier, which is the essence of developer experience. A perfect example is SRE teams improving observability to make handling incidents a breeze (well, that's the theory anyway...).
As I look into some of the new developer productivity tooling being released, I just can't help but think about the lessons I've learned as an SRE.

## What is Developer Experience?

For me, developer experience is about reducing friction and making it easy to get stuff done. By "get stuff done", I mean how hard is it to change the system? Does it take a long time? Do changes fail often? Do people enjoy working on the system? The DORA metrics provide a good way of measuring the first three. You'll have to talk to people for the latter I'm afraid. 
If the answers to the above are yes (or no for the last one), then the chances are your developer experience isn't great, and that's not just bad for the developers. Bad developer experience for software companies is like having a factory with old, broken machinery. You're not delivering anywhere near as fast as you could be, and your developers are getting hurt.

The good news is just like that factory can upgrade its broken-down machines, you can take steps to improve your developer experience. Yes, investing in better tooling will certainly help, but there are lessons we can take from SRE and software development in general, that go a long way to making your developer's lives easier.

## Keep it Simple
Unequivocally, the first lesson has to be "keep it simple". I apologise now if you were expecting some grand revelation, but it's something we've all been guilty of forgetting at one point or another.

Simple systems are simple to understand, simple to run, simple to debug, and usually simple to maintain. They're predictable, and one of the biggest frustrations and "friction points" as an engineer is working on a system that's so hard to reason about, it behaves unpredictably when you try to change it. 

KISS (keep it simple, stupid) needs to be applied at every layer, too. It might not sound exciting but simple architecture, simple (boring) code, and simple release processes are usually traits of a system that's a joy to work with, and great for developer experience. That's why we all love working on greenfield systems.

At this point in the post, I was going to list some practical ways you can keep things simple, but people have written entire books on the subject and at the end of the day, all I'm describing here is technical debt. At the risk of stating the obvious (and having a "Will Buxton moment" for the F1 fans), if you want to improve your developer experience the place to start is paying off your technical debt, and that doesn't mean your SonarCloud warnings either; although they are usually a smell.

## Observability
Another trait of systems with good developer experience is that they're easily "debuggable". This is different from having good monitoring; monitoring tells you that something is broken, and observability tells you why.

If we think about API response times, monitoring alerts you when it's high, and observability tells you that it's because one of the upstream services has a problem with its database, caused by yet another system hammering its API.

Not only does this help reduce the time to recover from incidents, which is great for DevEx, but there are other benefits, like helping new team members understand how the system fits together. Instead of cloning all the repos and scanning the code, which can take weeks to build up an accurate mental model, they can pull up a trace and understand which services are involved, the order they get called, and which ones are usually the bottleneck in mere minutes.

This is an area where good tooling helps. Functionally, you want something that can tie all your telemetry together to tell the story of what went wrong. Non-functionally, you need something fast and reliable, which is where the self-hosted solutions fail without a dedicated team.

## Automation, Deployments and Releases
We all know that manual deployment processes are bad for developer experience, and hopefully, we've all automated them by now (hah!). The impact on DevEx goes beyond automation though, it's possible to have automated pipelines that suck.

Inefficient pipelines are not only slow but also instil little confidence in the reliability of your deployed system. They're also impossible to run locally, and investing time in improving them generates an incredibly high return on investment over the system's lifetime.

Slow pipelines are slow to fail, which multiplies the cost of minor mistakes. Flaky pipelines or those lacking any tests at all result in firefighting situations and time wasted re-running test suites. As an SRE, one of my major frustrations stemmed from pipelines that couldn't be run locally. We've probably all experienced the absurdity of reaching "Release-31" on a new pipeline before it goes green. A practical solution to this problem is Dockerising pipelines.

We've talked about the implementation details, but it's also worth talking about release management and software delivery and its impact on DevEx. Developers want to release code. What I loved about being an SRE was identifying a problem, usually performance or reliability-related, working hard on a fix, getting the PR in, and then watching the latency or error rate graph plummet as the code got released. If I could inject that feeling into my veins I would. Now I couldn't have done that if I had to wait a month for my code to be bundled into a release, call me a child but when it comes to performance fixes delayed gratification isn't my thing. What enabled me to do that so quickly? Simple processes and continuous deployment. 

## Incidents
Hear me out: incidents can have a positive impact on developer experience.

OK, so I'm not going to argue that finding out your change just broke production is a great experience and everybody should do it, but an incident gives you a chance to run a post-mortem, and this is where you can ask deep questions about your entire process to improve things for the future.

One of my favourite things to do in a post-mortem is start right at the beginning, at the early requirements gathering phase, and ask questions like "What could we have done better?", "How could we have spotted this?", "How could we prevent this?", and then iterate those questions across each phase of the development process. You usually end up with a list of improvements that make certain processes easier, faster, or safer, which doesn't just prevent future incidents but also streamlines the process, which is the very definition of improving the developer experience.

Of course, that list is entirely useless if you never implement any of it.

## Summary

Developer experience and engineering enablement might be trendier now, but that doesn't mean it's new. Making it easier to build, ship and operate systems is what we've been doing as an industry forever, even if we do occasionally worsen things for a bit (early days microservices anyone?). 
Whilst I don't disagree that tools like Copilot and DevBox will improve developer productivity and the overall developer experience, I think it's important we remember the basics and get them right. You can buy all the tooling you want, but with a mountain of technical debt, systems that are horrendous to debug and work with, slow and fustrating release processes, and constant incidents that you don't take any learnings from, you're just sticking lipstick on a pig.









