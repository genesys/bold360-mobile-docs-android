---
layout: default
title: Trublesshooting 
parent: FAQ
nav_order: 1
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

### 👁️‍🗨️ Chat messages are trancated

If chat messages are displayed wrong: e.g. status is aligned to the wrong side, messages bubbles are cut, the bubbles are not sized properly,  check the `ConstraintLayout` version as was imported by your app. As of the last SDK version, it should be configured to at least version 2.0.4 (both on androidx and support library versions).
If you have the wrong version, and you're not actually importing this package, please add it to your imports with the proper version.
```gradle
implementation 'androidx.constraintlayout:constraintlayout:2.0.4'
```
or
```gradle
implementation 'com.android.support.constraint:constraint-layout:2.0.4'
```

Change didn't help?
{: .no_toc .light-title .mb-4 .mt-6 .notice}
  Try changing your current SDK version.   
  - If your SDK version is 4.0.6 or lower, you can upgrade to 4.0.7.
  - If your SDK version is 4.0.0 or upper, please upgrade at least to 4.3.0.


---

### ⚡ Unstable chat, outgoing messages failure 
SDKs outgoing requests are configured with a timeout period in which the response is supposed to be provided.
When the connection with the server is unstable, or the server becomes somewhat slow, you may experience requests failure on the chats outgoing messages.   

👉 First, verify that those failures are due to timeouts.   
Open the logcat logs, and look for the following log message:   
`ConnectionError: connection can't be established with destination: timeout`

If it is the case, there is an option to increase the timeout value, in order to let the server have more time to respond, before an error due to timeout is declared.   
The `requetTimeout` configuration is available on the [`ConversationSettings`]({{'/docs/chat-configuration/chat-settings#requests-timeout-since-430' | relative_url}}).   
Try setting a bigger value than the provided default.

```kotlin
conversationSettings.requestTimeout(TIMEOUT_IN_MS) // defaults to 30 sec.
```

