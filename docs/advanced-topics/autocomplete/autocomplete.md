---
layout: default
title: Autocomplete
parent: Advanced Topics
nav_order: 7
permalink: /docs/advanced-topics/autocomplete
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
Displaying suggestions for the user to select from, while the user types his message. The user than can select from the suggested articles/queries or continue typing his message.   
Selected suggestion will automatically be sent to the BE for response.
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

