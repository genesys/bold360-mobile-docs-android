---
layout: default
title: Chat Availability
parent: Tracking Events
grand_parent: Chat Configuration
nav_order: 2
# permalink: /docs/chat-configuration/tracking-events/chat-availability
---

# Chat Availability {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
Chat considered available, when the chat can be answered by a live agent.
Chat unavailability triggers state event `StateEvent.Unavailable`, which will have an `UnavailableReason` and will be passed to the hosting App along side unavailable form / message. 
{: .overview}

> If the chat is canceled by the user while in [`StateEvent.InQueue`]({{'/docs/chat-configuration/tracking-events/chat-lifecycle#inqueue' | relative_url}}) or while waiting for acceptance [`StateEvent.Pending`]({{'/docs/chat-configuration/tracking-events/chat-lifecycle#pending' | relative_url}}), chat end will be handled as `unavailable`.  
{: .overview}

---

## ChatAvailability API
### Availability check
Check if your chat is available.   

1. Create an `Account`.

2. Call `ChatAvailability.checkAvailability` as follows:
    ```kotlin
    ChatAvailability.checkAvailability(account, callback = object : ChatAvailability.Callback {
        override fun onComplete(result: ChatAvailability.AvailabilityResult) {
            // Validate no error
            // Check isAvailable and check the UnavailabilityReason if not 
        }
    })
    ```

    > `ChatAvailability.AvailabilityResult` provides the check parameters (apiKey, departmentId, etc) as well as the execution results 

{: .mt-5}
Availability check with department 
{: .strong-sub-title}
  
Use `ChatAvailability.checkAvailability` call as before, just add the departmentId.   
    
```kotlin
ChatAvailability.checkAvailability(account, departmentId, callback = object : ChatAvailability.Callback {
    override fun onComplete(result: ChatAvailability.AvailabilityResult) {
        // do your wonders...  
    }
})
```

{: .mt-5}
> ❗ In order to get availability status of the different departments in your organization, [make sure your api access key is not configured]({{'/assets/images/bold-console-apikey.png' | relative_url}}) to work with a specific department.

---

### Available departments
With `ChatAvailability` API, you can also fetch the current available departments, for your account configuration.

1. Create an `Account`.

2. Call `ChatAvailability.availableDepartments` as follows:
    ```kotlin
    ChatAvailability.availableDepartments(account, callback = object : ChatAvailability.DepartmentsCallback {
        override fun onComplete(result: ChatAvailability.DepartmentsResult) {
            // Validate no error
            result.error?.let{
                //handle error
            } ?:        
            // get departments list
            result.data.takeIf { it.isNotEmpty() }?.let{
                // handle departments list
            }
        }
    })
    ```

    > `ChatAvailability.DepartmentsResult` will contain, if no errors occurred, a list of `Department` items. Each contains name, id and language of the department.



