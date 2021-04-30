---
layout: default
title: TLS 1.2
parent: FAQ
nav_order: 3
# permalink: /docs/faq/tls12
---

# TLS 1.2 support
{: no_toc}

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---


## Overview
Transport Layer Security protocol.
{: .overview .fw-600}

Android devices can be in one of the following states: 
- Device does not have TLS 1.2 installed at all.
- Device has TLS 1.2 installed, but not enabled by default.
- Device has TLS 1.2 installed and enabled by default.   
{: .overview}

Since Bold systems are now under this security protocol, The Bold SDK as well was updated to support it.   
The Bold SDK enables the TLSv1.2 protocol on lower API (< 21) devices which are usually doesn't enable this option by default, even if installed.   
{: .overview}

---

## Verify TLS 1.2 is available on the device
_It is up to the hosting App to verify and suggest installation of TLS 1.2 protocol when is not installed on the device._

<u>Google play services</u> library, provides an easy way to do that:   
Using `ProviderInstaller.installIfNeeded` or `ProviderInstaller.installIfNeededAsync`   
> - Using Google play services to [activate latest security protocol installer](https://developer.android.com/training/articles/security-gms-provider)   
> - [How to activate the installer sample](https://github.com/bold360ai/bold360-mobile-samples-android/blob/master/SDKSamples/app/src/main/java/com/sdk/samples/MainActivity.kt)

**<u>Notice</u>**
> - If your App uses android **Support Library** - make sure you import the appropriate Google play version: `com.google.android.gms:play-services-base:16.1.0`
> - If your App uses [`androidx`](https://developer.android.com/jetpack/androidx) libraries, import the latest version.

