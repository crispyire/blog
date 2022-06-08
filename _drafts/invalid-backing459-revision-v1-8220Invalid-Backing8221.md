---
id: 460
title: '&#8220;Invalid Backing&#8221;'
date: '2016-06-21T13:03:20+01:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2016/06/459-revision-v1/'
permalink: '/?p=460'
---

Continuing on from my previous post about EVC Mode when migrating from old to new hardware and vCenter 5.1 to vCenter 6.0, I encounted another issue that you should be aware off with a VM network display ‘feature’ (OK… I mean bug).  
So as bit of background, I have been doing some work for a client migrating their VMware estate to vSphere 6.0 across 3 counties and 2 continants. As a financial organisation, they need to limit downtime to as near zero as possible and thus we opted to use a shuttle host (ESXi5.1, 1065491) to facilitate the live transfer of VMs to their new homes in vSphere 6.0 (vCenter 6.0, 3339084).  
I found as I pulled the shuttle host into the new vCenter withe the live VM payload that something very odd happened… I got VMs that reported that their vNICs had “Invalid Backing” (And it appears to be inconsitent of when it happens, as repeating the process didn’t produce the same results everytime (Roughly 25% of the time))

But connectivity was not interupted

This was a little more than a display bug as it stopped the VM from being vMotioned as the WebClient report it did not have a Source network and would not complete the vMotion

In the event that this issue appeared, it was possible to Edit Settings on the VM and change the network label to the correct one without any network connectivity loss. (This worked in this environment without issue, but please check connectivity in yours, in case of any further issues).  
Once the Network Label was correctly assigned, the vMotion happened with issue and the VM reported no further related issues.