---
layout: single
title:  "How to improve DevEx - Lessons from SRE"
date:   2023-12-07 20:30:00 +0000
tags: devex
classes: wide
header:
  og_image: /assets/img/posts/2023-12-01-azure-deployment-environments/DeployedEnvironment.png
---

I've been thinking about Developer Experience a lot recently, and something that really sticks out is the overlap with SRE. Like many companies, our SRE team was a finite resource, so we worked across the engineering organization. To get buy-in from other teams we had to demonstrate value, and by far the easiest way to do this was by making developers' lives easier, which is the essence of developer experience. You don't have to look much beyond the observability stack to see this, every improvement made it easier for engineers to respond to incidents (well, that was the theory anyway).

What is Developer Experience?
For me, developer experience is about reducing friction and making it easy to get stuff done. By “get stuff done”, I mean how hard is it to change the system? Does it take a long time? Do changes fail often? Do people enjoy working on the system? The DORA metrics provide a good way of measuring the first three. You'll have to talk to people for the latter, I'm afraid.

If the answers to those questions for your organization are yes (or no for the last one), then the chances are your developer experience isn't great.

Our industry loves a manufacturing metaphor, and in this case, a bad developer experience is like having a factory with outdated machinery (tools), unclear instructions (documentation), and inefficient processes (I'm sorry, I tried).

The good news is, just like that factory can upgrade its broken-down machines  (OK, I'll stop), you can also take steps to improve your developer experience.

Keep it Simple
This isn't specific to SRE, but unequivocally, the first point has to be “keep it simple”. I apologize now if you were expecting some grand revelation, but it's such a crucial aspect in improving developer experience and productivity.

Simple systems are simple to understand, simple to run, simple to debug, and simple to maintain. They're predictable, and one of the biggest frustrations and “friction points” as an engineer is working on a system that's so hard to reason about, it behaves unpredictably when you try to change it. It works the other way around too, sticking to the “predictable” way of doing things and avoiding the company/project specific customizations doesn't just make a simpler system, it makes developer onboarding so much easier. 

It's not just the code that needs to be simple either, it's the whole stack. Sure it's not exciting, but simple architecture, simple (boring) code, and simple release processes all add up to a system that's a pleasure to work with, and great for developer experience. That's one of the reasons we all love working on greenfield systems.

I was thinking about listing some practical ways you can keep things simple, but people have written entire books on the subject and at the end of the day, all I'm describing here is technical debt. At the risk of stating the obvious (and having a “Will Buxton moment” for the F1 fans), if you want to improve your developer experience, undoubtedly the place to start is paying off your technical debt. That doesn't mean your SonarCloud warnings either; I mean your real technical debt. 

I'd go as far as to say that for DevEx teams to have a serious impact, their primary focus should be technical debt. They should have a roadmap for strategically tackling complex and outdated parts of the architecture, and the parts of the system that aren't receptive to change. 

Observability
We mentioned this earlier, but another trait of systems with good developer experience is that they're easily “debuggable”. It's obvious that incidents and outages don't make for happy customers, but having the tools and data to resolve them quickly is the difference between happy and angry engineers.

Notice how I didn't use the word “monitoring” here? That's because monitoring tells you that something is broken, it doesn't tell you why. For that, your system needs to produce rich telemetry data in the form of events, and to send those events to a tool capable of presenting, analysing, and aggregating them into useful insights. 

Let's say you're monitoring API response times, and you get an alert when they go above 500ms. A system with good observability produces events whenever it does something, such as handling a request, performing some unit of computation, or IO. It also adds tags to those events containing rich contextual data, like the endpoint that was called, parameters, network information, and other domain specific values. If your tooling is capable of aggregating all of those events across the different dimensions (tags), you should be able to work out what is causing the spike in latency relatively easily.

Now, I can't imagine anyone reading this needs much convincing that the two are linked. Good developer experience means providing a repeatable, straightforward process for debugging incidents, and that requires good observability.

It's not just incidents that observability helps with either, it's a great tool for understanding how systems work and developer onboarding. You can use a trace to gain an accurate mental model of how a system works in minutes, opposed to days or even weeks combing through the code. That's not to say you can stop writing documentation, traces still don't tell you why a system works the way it does (ADRs, Architectural Decision Records, are great for that).

Automation, Deployments, and Releases
Another heavily SRE-related area that links to developer experience are your release and deployment processes. There's all manner of ways these can suck the life out of developers, even if they are automated.

Slow, or flaky pipelines are the obvious examples, even worse if it's both. Fixing these pipelines speeds up the developer workflow, prevents the frustration of having to re-run the tests to get them green, and also reduces the time it takes to resolve incidents. It's a bit of a no-brainer, but I suspect you already knew that.

When it comes to developer experience, there are two things that make me irrationally angry:

1. Pipelines that you can't run locally.

2. Not being able to release code.

If you've ever set up a pipeline from scratch, you know the absurdity of reaching something like “Release 151”  (99 of which are YAML indentation issues) before it actually goes green for the first time.  It sucks. 

It also sucks when it's an existing pipeline where some check is failing, but the error isn't clear, and you've got not way of replicating it on your own machine. 

There's a solution to both of these problems: Containerization. 

Creating your pipeline as a multistep Docker build makes it possible to run the entire pipeline end-to-end on your local machine, and simplifies the pipeline, just call `docker run`.

The second issue isn't so much an SRE issue, but a wider delivery issue, and the reason it effects developer experience is that developers like to see their code being used. 

As an SRE I loved nothing more than spotting a performance issue, working hard to understand the issue and create a fix, getting the PR in, and then watching the latency or error graph plummet as the code got released. If I could inject that feeling into my veins, I would. Now I couldn't have done that if I had to wait a month for my code to be bundled into a release, call me a child but when it comes to performance fixes, delayed gratification isn't my thing. 

Slow release processes can be demoralizing, and they often aren't necessary. There's little reason the vast majority of SaaS businesses can't implement continuous deployment all the way through to production.

Incidents
Hear me out: incidents can have a positive impact on developer experience.

OK, so I'm not going to argue that finding out your change just broke production is a great experience and everybody should do it. But an incident gives you a chance to run a post-mortem, and this is where you can ask deep questions about your entire process to improve things for the future.

One of my favourite ways to conduct a post-mortem is to iterate through each phase of the software development lifecycle, and explore how we might've prevented, mitigated, or detected the incident at that phase. It forces you to look beyond the surface level issues and really “shift left”. (I should really trademark the phrase “Shift-left post-mortems”). 

Now to bring this back to developer experience, many incidents occur because it's hard to do something earlier on in the development lifecycle, and it's not always testing. Using the post-mortem process helps you uncover what those things are, and so long as you complete the remediation actions, improves developer experience.

## Summary (Draft)

It's not as catchy, but we really should rename “DevEx” to “The Developer Experience”. The addition of that single word really emphases the meaning, developer experience covers every aspect of being a developer, and there's potential for friction and frustration at every turn. 

Good companies recognize this and the benefits improving it has not only on developers, but on broader organizational health. 

DevEx covers everything from developer onboarding, to can improve the developer experience, and as much as some tooling vendors would like you to believe, it's not just developer onboarding. For me, it always comes back to keeping things simple and predictable (Keep it simple), ensuring your developers have the capabilities they need (such as Observability), and iterative improvement (Incidents are a perfect reflection point). 

Obviously, there's more to capabilities and iterative improvement than just observability and incidents. “The Developer Experience” is a culmination of all the code, tooling, and processes that they encounter, and anything   but that was a pretty good way to wrap everything up, you have to admit.
