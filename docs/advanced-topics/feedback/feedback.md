---
layout: default
title: Feedback
parent: Advanced Topics
nav_order: 8
permalink: /docs/advanced-topics/feedback/
has_children: true
has_toc: false
---

# Feedback
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc .mb-0}
- [Instant Feedback](./instant-feedback)
- [Timed Feedback](./timed-feedback) 

---

## Overview
Statistic tool, enables to review the quality of AI response performance, knowledge base content and get some idea of customers needs in order to improve customer service.
{: .overview }

The Feedback feature is available only for AI chats.   

There are 2 types of feedback that can be configured, but only one can be active.

- [Timed feedback](./timed-feedback) - A configured periodic give feedback message, received as a response to an SDK timed feedback query to the BE.  
The Feedback applies on chat session as total.

- [Instant Feedback](./instant-feedback) - Each incoming message, that is available for feedback, is attached with an SDK's feedback component.   
The feedback applies to the specific response/article it was attached to.
