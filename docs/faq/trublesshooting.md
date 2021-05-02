---
layout: default
title: Trublesshooting 
parent: FAQ
nav_order: 7
permalink: /docs/faq/trublesshooting
---

# Trublesshooting
{: .no_toc}

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
{: .no_toc}
Here you can find solutions to common problems.
if you can't find your solution here, you can [post your issue](), and we'll be happy to help.
{: .overview }

---

### 👁️‍🗨️ Chat messages looks weired or cut

If chat messages are displayed wrong: e.g. status is aligned to the wrong side, messages bubbles are cut, the bubbles are not sized properly,  check the `ConstraintLayout` version as was imported by your app. As of the last SDK version, it should be configured to at least version 2.0.4 (both on androidx and support library versions).
If you have the wrong version, and you're not actually importing this package, please add it to your imports with the proper version.
```gradle
implementation 'androidx.constraintlayout:constraintlayout:2.0.4'
```
or
```gradle
implementation 'com.android.support.constraint:constraint-layout:2.0.4'
```
