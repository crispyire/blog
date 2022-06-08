---
id: 73
title: 'Everything is so…. Xtrem'
date: '2015-01-18T16:29:21+00:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2015/01/43-revision-v1/'
permalink: '/?p=73'
---

[![Xtremio](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/Xtremio.png?resize=300%2C112)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/Xtremio.png)

Over the last year I have been fortunate enough to work with a fantastic client in the Irish insurance market. They have taken the time and effort to not just look introspectively, but look to the future with a level of common sense that is refreshing.

As part of a major overhaul of their line-of-business infrastructure and software, they have looked at their storage and data requirements and have approached EMC to offer a proof of concept for an all-flash array.

As the title suggests they selected the EMC XtremIO all-flash array, and I was asked to provide the stability, performance and “is this what we need” tests for the company.

It was a very exciting day, on a very cold and wet autumn day in 2014 when the courier turned up with the boxes to the two datacentres and shortly afterwards an EMC installation engineer to complete the rack, stack and configure of the arrays.

Unlike other EMC arrays that I have seen and used, which have put a certain level of unnecessary glitz to their storage offerings (eg. Lots of blue neon light, big logos, etc)… The XtremIO is the opposite, it’s plain, it’s utilitarian, it’s… well… You think “is that it?” Don’t get me wrong, I prefer substance over glamour, but it was just a shock when it was in the rack next to the EMC VMAX.

[![Xtremio_front](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/Xtremio_front.png?resize=300%2C196)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/Xtremio_front.png)

After logging into the webconsole and setting up the FC LUNs, I set about doing some stress testing with some load testing VMs in the VMware ESXI5.1 environment. Unlike spinning disks, the normal testing procedures wouldn’t provide meaningful data, so it was advised by EMC that I use the Linux tools “btest” and “AFA Proof-of-Concept Toolkit”. As we wanted to put Oracle databases on the array, we also completed SLOB (The Silly Little Oracle Benchmark) tests.

Since performance is one of the key features of the Xtremio, we wanted to put it through its paces! What we found was that:

| **IOPs Range** | **Response Time** |
|---|---|
| 0 – 125,000 | Sub Millisecond (Microseconds) |
| 125,000-141,000 | 1 to 2 Milliseconds |

…At one stage I got 250,000 IOPS from this 10 flash-disk array! When I compare this to the EMC VMAX, it was clear whatever tests I was going to do to this thing, it wasn’t even going to touch the sides of its ability.

Two other features of the array are inline data duplication, and inline data compression. These features combined to give us times ten the amount of usable capacity than the disks alone could provide (This is a bit skewed as we didn’t have full production data on it during the tests). EMC was at pains to point out that it offers system level garbage collection as opposed to post-processing data reduction. This offers us near real-time gains in capability but without noticeable performance impact.

[![Xtremio_Console](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/Xtremio_Console.png?resize=300%2C167)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/Xtremio_Console.png)

Volume level snapshots are also a feature. The system offers atomic locked copy images of volume data with the state of the data captured at the specific point in time. This enables users to save the volume data state and then access the specific volume data whenever needed, including after the source volume has changed. As the snapshots are copies of an original source, it is deduplicated and thus the capacity footprint is very small (essentially the space taken up would just be the metadata).

[![Xtremio_Snapshot](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/Xtremio_Snapshot.png?resize=300%2C300)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/Xtremio_Snapshot.png)

As you can see from the above screenshot, the original volume is 400GB and the snapshot shows also 400GB but the actual space taken up on the array by the snapshot is 36.8GB!

The major shift from the existing infrastructure was the need to work out a method of replication between the two arrays. At present the EMC XtremIO does not offer any level of SAN based replication such as Symmetrix Remote Data Facility (SRDF).

It was decided to use the VMware host based replication method “VMware vSphere Replication” to cover this shortfall in capability. Though I’m assured it wasn’t on EMC’s roadmap to introduce such a feature, I hope they reconsider this in a future firmware release.

[![vSphere_Replication](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/vSphere_Replication.png?resize=300%2C95)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/vSphere_Replication.png)

Traditionally, storage arrays have always had complicated web interfaces which aren’t intuitive to say the least. There are some notable exceptions such as Dell Equallogic (But I’m biased, I used to work in the Equallogic team at Dell). The EMC XtremIO have a friendly, useful interface that gives all the necessary functionality without the need for the dark magic that seemed to be needed with other EMC or other vendors arrays.

Overall, this is an amazing bit of kit, and though I have not been able to test other vendors offerings if this is the future of arrays, the performance offered is mind blowing when put next to my experience of spin disk arrays. The power/performance can now be administered by server system administrators and the future of the dedicated storage administrator is now numbered.