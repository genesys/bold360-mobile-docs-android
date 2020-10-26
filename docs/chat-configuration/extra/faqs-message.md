---
layout: default
title: FAQs Message
nav_exclude: true
---

# FAQs Message
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview
AI chats can be configured to start with a FAQs chat message.   
The FAQs message consist of a generated list of FAQs, based on preconfigured rules defined on the [bold360ai console](https://support.bold360.com/bold360/help/how-to-set-up-the-dynamic-faq-widget).   
{: .overview}
The list of FAQs appears as an incoming chat message with a list of persistent options corelates to the FAQs, and links to their corresponding answers.   
The FAQs are fetched on chat start only. Continuance chats will not trigger FAQs reloading.
The FAQs message appears just beneath the [Welcome Message](./welcome-message), if configured.

![]({{'/assets/images/welcome-and-faqs.png' | relative_url}})
{: .image-40}

---

## How to configure
Configure on bold360ai admin console

![]({{'/assets/images/faqs-console.png' | relative_url}})
{: .image-70}

FAQs message will be fetched according to the `cnf` API response.
   
## How to disable
Delete all preconfigured FAQs lists on the admin console.

---

## Related Topics
 - [`Welcome message`](./welcome-message)