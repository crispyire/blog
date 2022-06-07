---
id: 789
title: 'Content Library &#8211; Cannot create service &#8220;%s&#8221;'
date: '2018-01-05T16:05:24+00:00'
author: 'Christian Parker'
layout: post
guid: 'http://www.baylyparker.com/?p=789'
permalink: /2018/01/content-library-cannot-create-service-s/
categories:
    - VMware
tags:
    - 'Content Library'
    - VCSA
---

Today I experienced an issue with a client’s site with them trying to deploy a VM from a vCenter Server Appliance (VCSA) 6.0. During the VM deployment wizard, they were getting the error **Cannot create service “%s”** and not being able to deploy any templates

I was able to reproduce the issue on different desktops and browsers so I logged into the VCSA and checked to see if all the services were running.

```
<pre class="brush: powershell; title: ; notranslate" title="">

service-control --list

```

..will give a list of all services available.

![](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2018/01/Service-List1.png?resize=300%2C281)

As the error relates to the VMware Content Library Service, I attempted to start that service and got the follow error (Which also includes part of the original error!)

![](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2018/01/Service-Failed1.png?resize=727%2C198)

There are a number of reasons why services will not start, but in this case it was due to the log mount (/dev/mapper/log\_vg-log) being full.

![](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2018/01/log-full1.png?resize=734%2C260)

The client wanted to retain the logs (Great use case for vRealize Log Insight!), so the VMDK that the mount pointed to was increased in space (Details on which VMDK and the process to do this can be found at <https://kb.vmware.com/kb/2126276>)

![](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2018/01/log-fixed1.png?resize=733%2C257)

After increasing the space, the issue was not resolved as the Content Library service hadn’t been cleared from the previous running state. To resolve this I followed <https://kb.vmware.com/kb/2147891> which started the service without issue.

![](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2018/01/Service-Start1.png?resize=730%2C55)

All templates in the Content Library could now be deployed without issue or error.