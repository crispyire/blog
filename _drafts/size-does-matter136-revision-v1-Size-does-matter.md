---
id: 270
title: 'Size does matter'
date: '2015-09-03T16:26:15+01:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2015/09/136-revision-v1/'
permalink: '/?p=270'
---

As the growth of virtualisation has become part of the day-today working environment of IT people’s lives, it has become so common place most of the general population aren’t even aware that their data/app/game/etc is living in the ‘cloud’. You can get into long and drawn out conversations about what the ‘cloud’ means but ultimately if it just works, the average person in the street doesn’t care. They just want to be able to access and update their information, play a game, do their work from anywhere, at any time (Needless to say this isn’t an exhaustive list, but you get my point).

There are many benefits of entering either a private cloud (You own all the hardware), a hybrid cloud (You own some hardware but you use some resources from a cloud provider) or a full public cloud (All the resources are offered by a third party cloud provider); one of the huge benefits is the scale and flexibility a cloud environment can offer businesses.

Scale is a relative term. I always narrow my eyes when I read text books, articles or heard people talk about a small, medium or large environment with the assumption by the author/speaker that audience knew what is meant. Even with experience it is relative to the situation. Two clients I help with their test and development sites couldn’t be more poles apart in terms of scale but for both they would consider their world ‘normal’. On one side I have one with three hosts, and a hundred virtual machines on a site and the other hundreds of hosts and tens of thousands of VMs. I think we can agree at least on this that one would be a ‘small’ environment and the other ‘large’ (Huge is a word I more often use).

When I wrote my last article I mentioned that VMware Professional Services (PSO) had been working with the larger client to rearchitecture their VMware solution. To that end, I thought I would write a little about what I have learnt.

The first is a simple one; it is important to know what the supported limits are. When I first started working with ESX4.1 and onwards, I honestly couldn’t tell you what the supported limits for many features. Why? Was I lazy? No, it was because in the wildest dreams of the majority of companies/organisations I was dealing with, they wouldn’t even come close to seeing these limits. vSphere 6 has a supported maximum of 64 hosts per cluster, many companies don’t have that many physical machines in their entire organisation, never mind in a single cluster. Knowing the limits of not just the individual products but any special considerations when they are combined, will help you avoid those out-of-hour calls and make your life easier if you do.

The second is that you need to make a mental shift between ‘Pets v. Cattle’. In all my previous work, I could name every virtual and physical server and knew at least on a basic level what they all did. When you scale up, names become meaningless and even trying to learn their names would be more akin to Sisyphus (The Greek mythology guy who was compelled to roll a boulder up a hill, only to watch it roll back down, repeating this forever.). Automation becomes king. Learning to script, using vRealize Automation (formerly vCloud Automation Center) and other methods to automate your life becomes less of a nicety and more of a necessity.

With automation, the watch word becomes ‘standardisation’. Your environment must have a consistent and repeatable configuration. Use the tools available to you… Host Profiles, Storage Profiles, Update Manager (VUM), etc. There are many other tools available both VMware and third party, but do make full use of them. Unless the foundations of your virtual world are stable, and consistent throughout, troubleshooting when things go wrong (And they will) will become traumatic at best.

The last point I want to make today is beware of organic growth and size of your environment. Ok, that’s unfair. Growth and controlled change is a good thing. What I mean is, regular review of what you have is important part of the life-cycle of the infrastructure. Just because some tool, hardware, software or other technology didn’t work or fit in the past, doesn’t mean it isn’t ideal now (or vice versa). If you got burned by using version 1.0 of a software, does that mean V9.3 isn’t going to give you nirvana?

I’m trying to think of the best way to sum this up in something short and snappy… I can’t really think of a five word sentence that will fix everything but I will say that reviewing your architecture regularly, keep learning from every source you can find and engage help if you need it (Your local [VMware Premier Solutions Provider](http://partnerlocator.vmware.com/)) will make your life easier.