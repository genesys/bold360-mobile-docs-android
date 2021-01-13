---
layout: default
title: Outgoing message
parent: UI Customization
grand_parent: Chat Configuration 
# permalink: /docs/chat-configuration/ui-customization/outgoing-message
nav_order: 4
has_toc: false
---

# Outgoing message {{site.data.vars.force-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview
The "User" side of the conversation. Usually this element is a request or a question from the user to the agent. The outgoing element can be, a typed user input, a user selection from a set of options, or a continuance information following a previous request.

#### Outgoing Ui component   
The outgoing ui element is configured and handled by `BubbleLocalViewHandler`.
This class can be overridden by extending **it** or extending `ChatElementViewHolder`.
If was overridden, the method `getLocalBubbleViewHolder` in [`ConversationViewsProvider`](ConversationViewsProvider#conversationviews) should be overridden and return the extending class.