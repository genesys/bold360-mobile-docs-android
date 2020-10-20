---
layout: default
title: Timed Feedback
parent: Feedback
grand_parent: Advanced Topics
nav_order: 1
permalink: /docs/advanced-topics/feedback/timed-feedback
---

# Timed Feedback
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
Chat sequence feedback, provides statistic information about user satisfaction of AI performance, during the chat. 
When enabled, an incoming message with feedback options, will be requested after a configured chat idle time. 
{: .overview}
The timed feedback message is actually the response to the SDK's feedback query passed to the BE after a configured time of no activity in the chat.

Timed feedback message is a regular incoming message and doesn't have special configurations. If you need to customize it, do it on [`IncomingElementUIProvider.customize`](/docs/chat-configuration/ui-customization/incoming-message/#how-to-customize) method, use the `element` property to check if responseType is `feedbackResponse`.

## Timed feedback configuration
Timed feedback can be enabled/disabled on the bold360ai admin console.   
The idle time period can also be configured once the feeback is enabled.

![](/assets/timed-feedback-console.png)
{: image-70}
