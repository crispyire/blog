---
id: 638
title: 'VM Storage Profiles on vCenter 5.5 &#8211; Noncompliant'
date: '2017-01-05T11:00:03+00:00'
author: 'Christian Parker'
layout: post
guid: 'http://www.baylyparker.com/?p=638'
permalink: /2017/01/storage-profiles-vcenter-5-5-noncompliant/
categories:
    - Storage
    - VMware
tags:
    - esxi
    - Profiles
    - storage
    - vmware
---

After implementing **VM Storage Profiles** this week at a client’s site, I noticed that even though I had added the correct storage profile to the virtual machine, and to the datastore, it was showing as ‘Noncompliant’.

![VM Storage Profiles](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2017/01/VM-Storage-Profiles.png?resize=300%2C81 "VM Storage Profiles")

On further inspection in the host profiles section of the c# client, I noticed that the ‘vm home’ (The location of the .VMX file) did not make itself compliant, even though the location was in the same place as its virtual hard drives (.vmdk files).

![VM Storage Profiles - vm home noncompliant](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2017/01/Noncompliant.png?resize=300%2C96 "VM Storage Profiles - vm home noncompliant")

After a lot of trial and error, I found the workaround was to Storage vMotion the VM to the same datastore (So go through the process of migrating the VM but selecting the same datastore as it currently resides).

![VM Storage Profile - Compliant](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2017/01/Compliant.png?resize=300%2C83 "VM Storage Profile - Compliant")