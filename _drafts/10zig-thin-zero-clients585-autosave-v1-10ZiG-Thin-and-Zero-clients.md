---
id: 609
title: '10ZiG Thin and Zero clients'
date: '2016-11-03T13:41:30+00:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2016/11/585-autosave-v1/'
permalink: '/?p=609'
---

In my [last post](http://www.baylyparker.com/2016/10/horizon-view-my-learning-journey/), I started to explain my re-interest in End User Computing (EUC) as I entered the world of Virtual Desktop Infrastructure (VDI). In this post, I wanted to look deeper into thin and  
zero clients combined with VDI.

After I had set up the VMware Horizon View infrastructure in my home lab; I was able to bring up a Windows 7 desktop on my Macbook Air. But though that demonstrated the mobility aspects that View can offer; it didn’t give me the insight into how many companies and organisations are using VDI in their workplace today. Namely, I wanted to see how VDI worked on zero and thin clients. I was lucky enough to be offered a couple to try out by 10ZiG Technology and it’s been a real insight in how a product can be made that embodies making life simple for the end user and the administrator alike.


### **10ZiG Technology![10zig_logo](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/11/10Zig_logo.png?resize=300%2C129)**

10ZiG Technology offer a huge range of thin and zero clients at are VMware, Citrix and Microsoft ready certified. The present product range includes [12 different clients](http://www.10zig.com/product/vmware-certified_thin_clients/) that are [VMware Ready Certified](http://www.vmware.com/resources/compatibility/) which is the focus of my blog today.

### **Zero and Thin Clients**

Firstly, what are zero and thin client? A zero client has a very cut down Operating System (OS) and no hard drive. The sole purpose of a zero client is to facilitate a connection to VDI. In contrast a thin client’s purpose is the same but also could include some essential applications natively/locally.

### Hardware or Software Zero Client

A hardware Zero Client is solely for use with PCoIP to give support for VMware Horizon View environments offering high-performance for CAD/3D etc users. A software Zero Client is a far more flexible device and allows for support of not only PCoIP, but also Blast Extreme, VMware’s new protocol. Although [VMware will continue support for PCoIP](http://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/techpaper/vmware-horizon-7-view-blast-extreme-display-protocol.pdf), in the future its reported that they will drop PCoIP for Blast. This means that the ‘Software Zero Client’ offers a greater level of flexibility and also future-proofed offering for VMware Horizon customers.

### **10ZiG V1200-P**

![v1200-p](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/11/v1200-P.jpg?resize=150%2C150)The [V1200-P](http://www.10zig.com/media/v1200-p_updated.pdf) I looked at, is a hardware zero client that connects via PCoIP to your View Connection Server. Based on Teradici PCoIP Zero Client which runs on the Tera2 chipset it seems to offer a fully featured VDI client. I could write about how it has dual screen capabilities, 4 USB ports and other features such as USB Smart Card options but what really impressed me was the zero setup time.

I did the basic setup of plugging in power, USB keyboard/mouse, a monitor and lastly a network cable and then it was time to turn it on. After less than a couple of seconds of hitting the power button, I’m prompted with the login screen to my View Connection Server! There was no time consuming setup, no install; just a login screen to my View setup. The ‘wow’ moment of out-of-the-box to running without any setup just blew me away.

### 10ZiG 5848qV

The [5848qV](http://www.10zig.com/media/5848qv_series.pdf) was the other zero client I was able to setup and review. Very much like the V1200-P, the setup was minimal, but it is designed for more multi-m![10zig-4548v](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/11/10ZiG-4448v.png?resize=150%2C150)onitor, full HD multimedia content for those VDI users that need more power such as 3D graphics or CAD users.

As the 5848qV is a Software Zero Client it offers a greater level of flexibility than its hardware client counterpart . Though I didn’t test this in my lab (A constraint in my lab environment) it also offers unlike the Teradici devices, wireless connectivity. It also supports Real-Time Audio Video (RTAV) for seamless Skype-for-Business and overcomes traditionally problems with running UC through VDI environments.

### Summing up

As I have spent more time with the 10ZiG hardware and Horizon, I have really appreciated what 10ZiG have created. At first, I really struggled to think about what I was going to write. After years of configuring full desktops, servers, storage, etc. that took hours or days to setup it was mind blowing to have a product that was full operational within seconds. So what was I going to write? “It just works!” seemed a very short blog post. As someone who has come with fresh eyes into the world of VDI, I just loved that simplicity. 10ZiG Technology have a range of solutions for most use cases, and I would urge you to give them a look at if you are either considering VDI, migrating from full to thin/zero clients or are considering another vendor.