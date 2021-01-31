---
layout: default
title: Autocomplete
parent: Advanced Topics
nav_order: 7
permalink: /docs/advanced-topics/autocomplete/
has_children: true
has_toc: false
---

# Autocomplete
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc .mb-0}
- [Autocomplete In chat](./in-chat)
- [Autocomplete Standalone component](./standalone) 

---

## Overview
Autocomplete refers to displaying completion suggestions while the user is typing a message. The suggestions are the most relevant text out of the phrases that exist in the knowledgebase. The user can select from the suggested text or continue typing his message.  
Suggestion selection triggers a selection event, that can be listened to and handled as needed.
{: .overview }


## AI autocomplete
_Autocomplete feature is available only for AI chats._
{: .fs-4 .fw-300 }

The presented suggestions are fetched from the BE while the user types his query.   

### Control availability
Autocomplete feature support status can be configured on bold360ai console.
- <details close markdown="block">
  <summary>Account level configuration</summary>

    ![]({{'/assets/images/autocomplete-account-console.png' | relative_url}}) 
    {: .image-70}

  </details>
{: mt-10em}
- <details close markdown="block">
  <summary>Widget level configuration</summary> 
    ![]({{'/assets/images/autocomplete-widget-console.png' | relative_url}})
    {: .image-70}

  </details>
{: mt-10em}
- [App side configuration](./in-chat)

