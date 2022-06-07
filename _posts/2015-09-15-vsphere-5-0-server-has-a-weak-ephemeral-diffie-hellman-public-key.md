---
id: 281
title: 'Server has a weak ephemeral Diffie-Hellman public key'
date: '2015-09-15T14:55:43+01:00'
author: 'Christian Parker'
layout: post
guid: 'http://www.baylyparker.com/?p=281'
permalink: /2015/09/vsphere-5-0-server-has-a-weak-ephemeral-diffie-hellman-public-key/
amazon-product-content-location:
    - '1'
amazon-product-excerpt-hook-override:
    - '3'
amazon-product-content-hook-override:
    - '2'
amazon-product-newwindow:
    - '3'
categories:
    - VMware
tags:
    - esxi
    - vmware
    - vsphere
---

I have just started to look into the decommissioning of a VMware vSphere 5.0 environment for a client as they prepare to upgrade to vSphere 6.0. When I tried to log into the webclient I was faced with this screen on Chrome V45 and similar in IE11:

<figure aria-describedby="caption-attachment-282" class="wp-caption aligncenter" id="attachment_282" style="width: 300px">[![Diffie-Hellman error](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/09/Diffie-Hellman.png?resize=300%2C232)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/09/Diffie-Hellman.png)<figcaption class="wp-caption-text" id="caption-attachment-282">Server has a weak ephemeral Diffie-Hellman public key ERR\_SSL\_WEAK\_SERVER\_EPHEMERAL\_DH\_KEY</figcaption></figure>

After a bit of research and a good pointer from [@sammcgeown](http://twitter.com/sammcgeown) I found that if I use an older browser (I tested a few but settled on [Firefox 37.0.1](https://ftp.mozilla.org/pub/mozilla.org/firefox/releases/37.0.1/win32/en-GB/)), the error message went away and was replaced by a new issue:

<figure aria-describedby="caption-attachment-283" class="wp-caption aligncenter" id="attachment_283" style="width: 300px">[![RSL 2046](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/09/RSL2046.png?resize=300%2C265)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/09/RSL2046.png)<figcaption class="wp-caption-text" id="caption-attachment-283">RSL Error Error #2046: The loaded file did not have a valid signature.</figcaption></figure>

The issue is outlined in [KB 2116567 The vSphere Web Client 5.0 fails to load and reports the error: Error # of 29 2046 with RSL](http://kb.vmware.com/kb/2116567)

As there is no solution to the issue as outlined in the KB article, but I found that if you set your clock back (I changed it to Jan-2015), the issue is resolved.

[![vSphere 5.0 Login](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/09/vSphere50.png?resize=300%2C201)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/09/vSphere50.png)

There are some blog posts on how to fix the certificates but for a quick and dirty solution I hope you find this helpful!