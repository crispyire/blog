---
id: 551
title: 'VMware Horizon View, My learning Journey'
date: '2016-10-14T14:48:06+01:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2016/10/543-revision-v1/'
permalink: '/?p=551'
---

![Horizon View](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/10/view.jpg?resize=292%2C173)Over the last few years my career has increasingly been divorcing me from end user computer (Desktop) support. I have been designing, deploying and supporting SDDC solutions across multiple industries and countries. After leaving [Microsoft Premium Support](http://ie.linkedin.com/in/crispyire/), I thought I had left end user support forever, as I had moved onto enterprise storage and server virtualisation.

I was asked recently, to be onsite for an international bank to help with a number of projects to increase their infrastructure resilience and data centre migration. It became increasing evident that Virtual Desktop Infrastructure (VDI) solution was a very good fit for them. After an initial conversation, it was clear they thought so too… Time for a POC!

As a certified [Microsoft Certified Solutions Expert (MCSE)](https://www.microsoft.com/en-ie/learning/mcse-certification.aspx), I’m no stranger to the OS side of physical desktop (and server) world, but I wasn’t up to the speed in terms of VDI. That is the point, I decided to hit the books, PDFs, eLearning, Labs and learn VMware’s implementation of VDI; VMware Horizon View.  
One of the best ways I have found of learning new technology is taking a certification path to absorb that knowledge. The[ framework and blueprints](https://mylearn.vmware.com/mgrReg/plan.cfm?plan=64299&ui=www_cert) have been a great answer to “Where do I start?”.

Using all the resources available to me I have focused on getting my [VCP6-DTM](https://mylearn.vmware.com/mgrReg/plan.cfm?plan=64296&ui=www_cert) certification, so I can get into the world of VDI and help my client in the best possible way. In many ways the certification itself is not as important as the learning journey that I have been through to get the knowledge/skills.  
After building out my home lab with a DNS/AD/DHCP 2012 server and vCenter 6.x (A blog for another time); it was time to set about the new adventure. Once the View Connection Server was deployed, the learning journey real did start.

This was the time, I found out how important I really needed to combine all my VMware, Windows and networking skills to get a durable environment working. Learning for me, means making mistakes and learning from them…

So what I learnt was the basic foundation needs to be solid and stable for VDI as with any virtual infrastructure (Don’t make assumptions). There was also product specific learning to get the foundations right. This came together for me in that I had a limitation in my lab, that my DNS server was hardcoded into the ISP DHCP server. As I had a separate DNS Server for my lab this became an issue as the View Agents communicate back to the View Connection Server via DNS names. As my previous home lab VMs had always had static IP/DNS or IP Pools this was never an issue before. After a number of failed deployments and a lot of head scratching, my desktop VMs deployed, customised and were ready for connection via the View Client.

I was very excited when I connected to the Horizon View Client, my Desktop Pool appeared and my Windows Desktop appeared on my Mac. I promptly tweeted my success and then went over the last couple of weeks about seeing how to tearing it down, breaking it, fix it and see its other capabilities. I really wanted to get VMware NSX installed in the lab and see how all the greatness of NSX can be used to secure VDI workloads. NSX Manager was deployed that weekend!

![View with 10ZIG](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/10/VDI.jpg?resize=150%2C150)Shortly after I shared my personal success with the world via Twitter, [10Zig Technology](http://www.10zig.com/) contacted me to ask if I wanted to test some of their thin and zero clients. I was very honoured to be asked, and jumped at the chance to broaden out my new found skillset onto some cool tech.

I will share with you in my next blog about how I got on with the thin and zero clients.