---
layout: default
title: JCenter Deprecation
parent: FAQ
nav_order: 1
# permalink: /docs/faq/pod-update-version
---

# JCenter Deprecation

>JCenter deprecation and end of service

JFrog, the company that maintains the JCenter artifact repository used by many Mobile projects, recently [announced](https://jfrog.com/blog/into-the-sunset-bintray-jcenter-gocenter-and-chartcenter/) the deprecation and upcoming retirement of JCenter.
Bold SDK artifacts were located on this repository as well. 
{: .overview}
**We moved our artifacts to a new host. Old and new versions of Bold360 SDK are available on this host.** 

In order to get our artifacts from the new host replace the following repositories links in the app's `gradle.build` file:

Replace:
- maven { url 'https://dl.bintray.com/bold360ai-sdk/core/'}
- maven { url 'https://dl.bintray.com/bold360ai-sdk/conversation/'}
{: .removed}

With:
+ maven {url "https://bold360ai-mobile-artifacts.s3.amazonaws.com/android/release/"}
{: .added}


In the near future, we will provide additional information, please follow release notes.
{: .mt-8 .overview}