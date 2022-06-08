---
id: 4540
title: 'vCenter 6.7 to 7.0 upgrade on MacOS'
date: '2020-05-26T22:38:52+01:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2020/05/4534-revision-v1/'
permalink: '/?p=4540'
---

While recently, upgrading vCenter 6.7 to 7.0 from my MacOS Catalina (10.15.4) laptop, I ran into an error while trying to complete the upgrade via the Installer UI

<figure class="wp-block-image size-large is-resized">![](https://i2.wp.com/www.baylyparker.com/wp-content/uploads/2020/05/Screenshot-2020-05-26-at-17.58.56.png?fit=1024%2C425)</figure>Normally, I would look in the Security &amp; Privacy settings to see that the application had been blocked, and an option to approve it… It wasn’t there

<figure class="wp-block-image size-large is-resized">![](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2020/05/System-Preferences.png?resize=336%2C287)</figure> After searching without success on the internet, I found [William Lam](https://www.virtuallyghetto.com/2020/02/how-to-exclude-vcsa-ui-cli-installer-from-macos-catalina-security-gatekeeper.html)‘s blog on a similar subject, and tried to put the suggestion to the vCenter 7.0 installer, but I had no success. I found that I could only get the installer to work, when I had:

1. Unpacked the ISO to a folder
2. Moved the folder from the Downloads folder to another location

<figure class="wp-block-image size-large">![](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2020/05/Screenshot-2020-05-26-at-22.15.55.png?resize=572%2C151)</figure>```
<pre class="wp-block-preformatted">sudo xattr -r -d com.apple.quarantine ./VMware-VCSA-all-7.0.0-16189094.iso
```

…as you can see from the screenshot, the first command gave no errors, but the installation failed at the same point. The second command worked once the folder had been moved out of the Downloads location.