---
id: 4534
title: 'vCenter 6.7 to 7.0 upgrade via MacOS'
date: '2020-05-27T13:00:00+01:00'
author: 'Christian Parker'
layout: post
guid: 'http://www.baylyparker.com/?p=4534'
permalink: /2020/05/vcenter-6-7-to-7-0-upgrade-on-macos/
ct_ignite_last_updated:
    - default
spay_email:
    - ''
categories:
    - VMware
tags:
    - '6.7'
    - '7.0'
    - upgrade
    - vCenter
---

While recently, upgrading vCenter 6.7 to 7.0 from my MacOS Catalina (10.15.4) laptop, I ran into an error while trying to complete the upgrade via the Installer UI

<figure class="wp-block-image size-large is-resized">![](https://i2.wp.com/www.baylyparker.com/wp-content/uploads/2020/05/Screenshot-2020-05-26-at-17.58.56.png?fit=1024%2C425)</figure>```
<pre class="wp-block-preformatted">"ovftool cannot be open because the developer cannot be verified. macOS cannot verify that this app is free from malware"
```

Normally, I would look in the Security &amp; Privacy settings to see that the application had been blocked, and an option to approve it… It wasn’t there

<figure class="wp-block-image size-large is-resized">![](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2020/05/System-Preferences.png?resize=336%2C287)</figure> After searching without success on the internet, I found [William Lam](https://www.virtuallyghetto.com/2020/02/how-to-exclude-vcsa-ui-cli-installer-from-macos-catalina-security-gatekeeper.html)‘s blog on a similar subject, and tried to put the suggestion to the vCenter 7.0 installer, but I had no success. I found that I could only get the installer to work, when I had:

1. Moved the ISO from the Downloads folder to another location before mounting it
2. Run the following command

```
<pre class="wp-block-preformatted">sudo xattr -r -d com.apple.quarantine ./VMware-VCSA-all-7.0.0-16189094.iso
```

<figure class="wp-block-image size-large">![](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2020/05/Screenshot-2020-05-26-at-22.15.55.png?resize=572%2C151)</figure>…as you can see from the screenshot, the first command gave no errors, but the installation failed at the same point. The second command worked once the ISO had been moved out of the Downloads location. The installation to vCenter completed without issue.