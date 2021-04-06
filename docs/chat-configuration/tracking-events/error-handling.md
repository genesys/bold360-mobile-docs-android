---
layout: default
title: Error Handling
parent: Tracking Events
grand_parent: Chat Configuration
nav_order: 4
# permalink: /docs/chat-configuration/tracking-events/error-handling
---

# Error Handling {{site.data.vars.force-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview

Most of the SDK's errors are being thrown by the ChatEventListener's `onError` method.

## Error Codes

The following list contains the SDK error codes.

- ### General Errors

  Errors that can be thrown for all the chat types - server exceptions

  - GeneralError Some Error
  - AccessControlError requested access (to a critical system resource such as the file system or the network) is denied
  - MissingRequestParamsError
  - ServerError
  - ClientError
  - ConnectionException
  - TimeOutError
  - NotFoundError
  - ForbiddenError
  - EmptyError - empty/null data
  - ExceptionError - General exception

- ### Bot Errors

  Bot chat specific errors codes

  - ConversationCreationError - unable to create conversation

    possible reasons:
    - NotEnabled - Chat with the provided account is not supported
    - EmptyResponse - empty/null response
    - NotAvailable - connection problem
    - IllegalStateError - account type is not recognized/other

  - StatementError - unable to send statement
  
    possible reasons
    - ConversationNotFound - connection problem
    - MissingRequestParamsError
      - NRConversationMissingEntities are missing
      - Chat can't be activated due to missing apiKey

  - ConfigurationsError - unable to fetch configuration response
    passible cause
    - EmptyError - Failed to load configurations

  - TrackingError

- ### Live/Async Errors

  Bold chat specific errors

  - ChatCommunicationError - There was a communication problem with the chat server
  - InvalidChatMessage
  - FormSubmissionError
  - TemporaryDisconnection - disconnected attempt reconnection
  - LanguageChangeError

  - PERMISSION_ERROR(404)
    possible reasons
    - IllegalStateError - BoldChat should be prepared with valid apiKey
    - internet permission is not available

  - ChatStartError(503)
     possible cause
    - MalformedAccessKey - access key issue


- ### Async specific Errors

  Async chat specific errors

  - MalformedAccessKey

### Other Error pathes

- **File Upload errors** - see the[file upload](/docs/advanced-topics/file-upload) documantation