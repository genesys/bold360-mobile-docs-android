---
layout: default
title: readmore Article
np_toc: true
nav_exclude: true
---

# readmore Article 
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
Article page is presented when the `read more` indication is selected on the incoming message.
The article's channels and feedback components are also available on the displayed screen.
{: .overview}

## UI configurations
The SDK provides a configurable _Article Screen_ implementation.
Default configuration can be changed using `ChatUIProvider.articleUIProvider.articleUIConfig`.
> Currently only customization by configuration changes is available. 

```kotlin
val customProvider = ChatUIProvider(context).apply {
    articleUIProvider.apply {
        // Configure screen close button
        articleUIConfig.closeUIConfig = CloseUIConfig(...)

        channelsUIProvider = ... // A QuickOptionUIProvider object
      
    }
}

ChatController.Builder(context).apply{
            //... do some initiations
            chatUIProvider(customProvider)
        }
```

### Article `Close` button <sub>Will be available from version 4.2.1</sub>
By default a close button will be available on the top right of the screen. 
Button configurations are defined by `CloseUIConfig` and are available for changes.

```
articleUIConfig.closeUIConfig = CloseUIConfig(Context).apply {
            position = ...  // Alignment setting according to UiConfigurations.Alignment options
            drawable = ....  // A DrawableConfig object, setting the drawable to display
            closeText = ... // close text, in case we want to display a text along side the image

            // >> for text only set drawableConfig to null.
        }

```

Hiding `Close` screen button
{: .strong-sub-title}

```kotlin
 val customProvider = ChatUIProvider(context).apply {
      articleUIProvider.articleUIConfig.closeUIConfig = null // Close button will ne be displayed on article screen
 }
```

### Article fonts and backgrounds configuration

```kotlin
articleUIConfig.closeUIConfig = CloseUIConfig(Context).apply {

    // adjust article content top, bottom marigns 
    articleUIConfig.verticalMargin = x to y // Pair of top, bottom margin values, in pixels

    // Configure the main background of the Article
    background = ....

        // Configure the article title
    title.apply {
        font = ... // A StyleConfig object
        background = ... // A color int
    }

    // Configure the article body
    body.apply {
        setStyle( .. ) // Set the font using its font family (capable with css)
        background = ... // A color int
    }
}
```
