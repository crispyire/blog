---
id: 642
title: 'Storage Profiles on vCenter 5.5 &#8211; Noncompliant'
date: '2017-01-04T11:50:22+00:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2017/01/638-revision-v1/'
permalink: '/?p=642'
---

After implementing **VM Storage Profiles** this week at a client’s site, I noticed that even though I had added the correct storage profile to the virtual machine, and to the datastore, it was showing as ‘Noncompliant’.

![VM Storage Profiles](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2017/01/VM-Storage-Profiles.png?resize=300%2C81 "VM Storage Profiles")

On further inspection in the host profiles section of the c# client, I noticed that the ‘vm home’ (The location of the .VMX file) did not make itself at compliant even though the location was in the same place as its virtual hard drives (.vmdk files).

![VM Storage Profiles - vm home noncompliant](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2017/01/Noncompliant.png?resize=300%2C96 "VM Storage Profiles - vm home noncompliant")

After a lot of trial and error, I found the workaround was to Storage vMotion the VM to the same datastore (So go through the process of migrating the VM but select the same datastore as it currently resides).

![VM Storage Profile - Compliant](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2017/01/Compliant.png?resize=300%2C83 "VM Storage Profile - Compliant")