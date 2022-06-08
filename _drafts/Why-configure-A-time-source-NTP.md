---
id: 5825
title: 'Why configure: A time source (NTP)?'
date: '2020-09-18T14:38:57+01:00'
author: 'Christian Parker'
layout: post
guid: 'http://www.baylyparker.com/?p=5825'
permalink: '/?p=5825'
ct_ignite_last_updated:
    - default
categories:
    - VMware
---

In the first of a series of blogs, I look into the need for certain configuration items within VMware vSphere.

<div class="wp-block-image"><figure class="alignright is-resized">![How the world became obsessed with time and efficiency | National Post](https://nationalpostcom.files.wordpress.com/2018/01/timeobsessed.jpg?quality=100&strip=all&w=642&resize=354%2C266)</figure></div>The first is making sure there is a consistent time source across your infrastructure. Does it matter and what breaks (if anything), if you don’t.

The management components of vSphere and the core infrastructure in most cases need to be configured with a time that is the same (Within a range). Not having the same/similar time can result in vSphere either not working, getting inconsistent results, or will hind troubleshooting.

There are many features in vSphere that rely on the same time for components to communicate with each other. From Active Directory integration allowing centralized authentication to vCenter to certificate handshaking between Platform Service Controllers (PSC)

**Active Directory**

Kerberos is a computer network authentication protocol that is the basis of authenticating Active Directory tokens. Kerberos may not work if the skew is more than five minutes (300 seconds).

<https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/maximum-tolerance-for-computer-clock-synchronization>

**External PSC or multiple PSCs**

If your PSC is external to vCenter or there are multiple PSCs in the environment, the time source must also be in sync.

Though [KB 75032](https://kb.vmware.com/s/article/75032) relates to the deployment/upgrade, it shows the need for a synchronised time source.

**Certificates**

When the VMware Certificate Authority (VMCA) is in used, a synced time source is a requirement. The [VMware Doc](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.psc.doc/GUID-DE49FBF5-E24A-462B-91DC-C4284D93F654.html) outlines the requirements.

**Logs**

When troubleshooting vSphere issues, reviewing logs is the cornerstone of any review. Matching logs from different vSphere components (eg. ESXi and vCenter logs) becomes a very difficult task when the timestamps of the logs are out of sync.

**Resolution?**

Though you can manually set the time, this has a high administrative overhead. The simplest and in my opinion, the easiest solution is to use a centralized time source such as a Network Time Protocol (NTP) server.

NTP is a networking protocol for clock synchronization between computer systems. They come in two flavors (Public and Private). A public NTP server is a service that is available for public consumption. Some are run by government agencies such as the National Institute of Standards and Technology ([NIST](https://www.nist.gov/pml/time-and-frequency-division/services/internet-time-service-its)), volunteer organizations such as Network Time Protocol project ([ntp.org](http://www.ntp.org)), and other commercial organizations (eg. Microsoft). A private NTP server is one that is within the local network and there for the sole use of an organization (or those it explicitly allows).

**Design Configurations**

My preferred NTP location is a public NTP server. In many designs though, there are security considerations that mean infrastructure servers/devices do not have direct access to the internet. An option would be a dedicated NTP appliance gaining its time from Global Positioning System (GPS) data but these can often be costly and only in a few edge cases reach the threshold that its benefits exceed their cost.

Where the client requirements are to limit public-facing network traffic, it would be advisable to have a local NTP server (Physical or Virtual). All devices/servers that are NTP capable should be pointed at this single source of truth for time. It would be suggested that the NTP server does have access to a public NTP server to ensure no time drift.

Lastly, to reduce the single point of failure that a lost NTP server may create, another NTP server should be configured. As many devices/servers that offer connection to NTP servers are hierarchical by nature you can define more than one NTP server in the configuration. The configuration is a primary and secondary setup. Therefore if the primary is uncontactable, the device/server will attempt to connect to the secondary.

<https://docs.vmware.com/en/VMware-Validated-Design/5.1/sddc-planning-and-preparation/GUID-39743023-3139-4289-A2EC-C40DCC3AFB76.html?hWord=N4IghgNiBcIHYBcAOIC+Q>

**Further Reading**