---
layout: default
title: Article page 
np_toc: true
nav_exclude: true
---
# Article page <sub>readmore</sub> 
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
        articleUIConfig.apply { // screen's title and body configurations 
            ...
        }
        channelsUIProvider.apply { // Bottom channels UI configurations
            ...
        }
    }
}
ChatController.Builder(context).apply{
            //... do some initiations
            chatUIProvider(customProvider)
        }
```

### Article configurations
{: .mt-8}
Article available configurations defined by `ArticleUIConfig`. This class defines the available UI configuration properties for the article page.  
Configured properties will override any configured font or color which were defined on the article source itself. e.g. If the article was configured to display the content with some kind of font, and a font was configured on the articleUIConfig.body.setStyle(...), this font will take precedence.

```kotlin
val customProvider = ChatUIProvider(context).apply {
    articleUIConfig.apply {
        
        // Defines screen close button configurations:
        closeUIConfig = CloseUIConfig(...)

        ///////////// Available from version 4.5.1: ///////////////

         // Adjust the page's paddings
        setPadding(...)

        // Adjust article content paddings
        setContentPadding(...)

        ///////////// Available from version 4.4.0: ///////////////
        
        // Adjust article content top, bottom marigns
        // >> Got Deprecated on 4.5.1 << 
        verticalMargin = x to y // Pair of top, bottom margin values, in pixels
        
        // Configure the page's background:
        background = .... // Drawable, e.g. ColorDrawable(Color.BLUE) or ContextCompat.getDrawable(context, R.drawable.bg)

        // Configure the article's title:
        title.apply {
            font = ... // A StyleConfig object, e.g. StyleConfig(16, ContextCompat.getColor(context, R.color.color_def), Typeface.DEFAULT)
            background = ... // Drawable, e.g. ColorDrawable(Color.RED) or ContextCompat.getDrawable(context, R.drawable.title_bg)
        }

        // Configure the article's body:
        body.apply {
            // Defines the font configurations for the article body:
            // Sets font's size in px, color, fontFamily and fontStyle.
            // e.g. setFont(14.px, Color.WHITE, "monospace", Typeface.ITALIC)
            setFont( .. )
            background = ... // Int color, e.g. Color.BLACK
        }
    }
}
```

### Article `Close` button
{: .mt-8}
By default a close button will be available on the top right of the screen. 
Button configurations are defined by `CloseUIConfig`.

```kotlin
val customProvider = ChatUIProvider(context).apply {
    articleUIConfig.closeUIConfig.apply {
        position = ...  // Alignment setting according to UiConfigurations.Alignment options

        drawable = ....  // A DrawableConfig object, setting the drawable to display. null, for text only display.

        closeText = ... // close text, in case we want to display a text along side the image

        // Defines the paddings of the Close Button:
        setPadding(...)

        // Defines margins of the Close Button
        setMargin(...)
    }
}
```

> Hiding `Close` screen button display, can be done by setting it's configuration to null.
  ```kotlin
  val customProvider = ChatUIProvider(context).apply {
      articleUIProvider.articleUIConfig.closeUIConfig = null
  }
  ```

### QuickOptionUIProvider
{: .mt-8}
Defines the channels display at the article page bottom.   
Follow [quick options configuration]({{'/docs/chat-configuration-ui-customization/incoming-message/incoming-options#QuickOptions' | relative_url}}).


