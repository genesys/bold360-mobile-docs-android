---
layout: default
title: Url Links 
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

## Overview
Links in chat messages are presented to the user as such, and enables user selection.   
The SDK does not handle the action that follows the links selection, it sends the link to the hosting app to further handling. The SDK doesn't force specific action but lets the hosting app to develop its own processing.
{: .overview}

## Listening to url links selection
The hosting app can listen to url links selections by implementing the `ChatEventListener.onUrlLinkSelected` method.   

<a id=accessibility />
The app can react to links selections how ever it see fits.    
The app can conditioned the url link open by user action.   
> Optional usage can be, **announcing with the [accessibility]({{'/docs/faq/accessibility' | relative_url}}) service API** of the selected url, and opening it if user will do a certain action.

### Linked article
The SDK does not handle url links opening, unless it's an article link.   
Article links which are embeded to bot articles with a special format, are activated automatically by the SDK and actually passes a request for the selected article.

---

## In-app links 
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
