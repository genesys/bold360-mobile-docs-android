---
layout: default
title: Links
parent: FAQ
nav_order: 9
---

# Chat Links
{: .no_toc}

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Chat url links activation
Hosting app can listen to url links selection by implementing `ChatEventListener.onUrlLinkSelected` method.

### Accessibility link annoncments
The App can control the Link announcements after the link click when listennting to link selection.

### In-app links 
In-app links presses are handled by the SDK.

How to configure in-app navigation channels on Bold360ai console
{: .strong-sub-title}

1. Navigate to Channeling -> Channeling Policy

2. Create a new Channel.

3. Fill up the next fields:
    * Channel name and description
    * Who’s the target audience
    
4. At `What To Do` select `Ticket`
    
5. Fill the relevant fields:
    * Label
    * Button action -> `Open Custom URL`
    * Fill the Link Url with the inApp navigation prefix (example: inApp://MainFragment)
    
6. Click on Save Settings
