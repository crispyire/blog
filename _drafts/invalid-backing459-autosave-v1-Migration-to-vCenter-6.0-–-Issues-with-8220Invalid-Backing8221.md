---
id: 472
title: 'Migration to vCenter 6.0 – Issues with &#8220;Invalid Backing&#8221;'
date: '2016-06-23T11:46:52+01:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2016/06/459-autosave-v1/'
permalink: '/?p=472'
---

Continuing on from my [previous post about EVC Mode](http://www.baylyparker.com/2016/06/migration-vcenter-5-1-6-0-issues-evc-mode/) when migrating from old to new hardware and vCenter 5.1 to vCenter 6.0, I encountered another issue that you should be aware off with a VM network display ‘feature’ (OK… I mean bug).

So as bit of background, I have been doing some work for a client migrating their VMware estate to vSphere 6.0 across 3 counties and 2 continents. As a financial organisation, they need to limit downtime to as near zero as possible and thus we opted to use a shuttle host (ESXi5.1, 1065491) to facilitate the live transfer of VMs to their new homes in vSphere 6.0 (vCenter 6.0, 3339084).

I found as I pulled the shuttle host into the new vCenter withe the live VM payload that something very odd happened… I got VMs that reported that their vNICs had “Invalid Backing” (And it appears to be inconsistent of when it happens, as repeating the process didn’t produce the same results every time (Roughly 25% of the time))

![VM Invalid Backing](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/06/VM-InvalidBacking.png?resize=692%2C253)

But connectivity was not interrupted.

![Ping](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/06/Ping.png?resize=562%2C133)

This was a little more than a display bug as it stopped the VM from being vMotioned as the WebClient report it did not have a Source network and would not complete the vMotion.

![vMotion Network Issue](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/06/Vmotion-Network.png?resize=869%2C161)

In the event that this issue does appear, it was possible to Edit Settings on the VM and change the network label to the correct one without any network connectivity loss. (This worked in this environment without issue, but please check connectivity in yours, in case of any further issues).

Once the Network Label was correctly assigned, the vMotion happened without issue and the VM reported no further related issues.