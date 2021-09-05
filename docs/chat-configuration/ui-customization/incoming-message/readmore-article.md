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
Article page opens up when the `read more` indication is selected on the incoming message. The purpose of that screen is to display the full article, as it is cut on the chat bubble.  
{: .overview} 
The article page displays the article's title, content, feedback and channels.   
Feedback and channel selection are functional on the full article display as well.

## UI configurations
Basically, the article is displayed on a WebView as an HTML page. All the tags, styles, fonts, colors, etc, that were added to the article content to configure how it is presented to the user, when edited on the Console editor, are available on the mobile display as well. On the SDK we enables set and override of article display properties.

The article page configurations (defaults to the console edited version) can be set using `ChatUIProvider.articleUIProvider`.

> 👉 At this point, the article page is configurable but **not** overrideable. 

`ArticleUIProvider` contains `ArticleUIConfig` for page configuration settings, and a [`QuickOptionUIProvider`](#quickOptionUIProvider) property, if the channels display should be different than in the chat.
The [feedback component]({{'/docs/advanced-topics/feedback/instant-feedback/#instant-feedback-configurations' | relative_url}}) will have the same style as was configured to it on the chat display.

Passing article page configuration to the SDK is done as follows:
{: .sub-strong-title}

```kotlin
val customProvider = ChatUIProvider(context).apply {
    articleUIProvider.apply {
        articleUIConfig.apply { // title, body, and general page configurations 
            ...
        }
        channelsUIProvider.apply { // Channels list UI configurations
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
{: .mt-8 .mb-4}
Article page configuration can be set by `ArticleUIProvider.articleUIConfig`.  
> Value of null, will display the article as designed on the console editor.

##### 📚 What can be configured
{: .notice .mt-6}

- General page properties, such as, padding, margins, background.

- [`CloseUIConfig`](#article-close-button) - Defines configurations for the page close button. Image, padding, margins, location, etc.   
  - Set to `null` to prevent its display.   
  - Can be configured to display image, text, or both.

- `TitleUIConfig` - Defines background and font style configurations, for the article's title.

- `BodyUIConfig` - Defines background and font style configurations, for the article content.

{: .det}
<details close markdown="block">
<summary> Extra details... </summary>

```kotlin
val customProvider = ChatUIProvider(context).apply {
    articleUIConfig.apply {
        // Defines screen close button configurations:
        closeUIConfig = CloseUIConfig(...)

        // Since 4.5.1 - Adjusts the page's paddings
        setPadding(...)

        // Since 4.5.1 - Adjusts article components paddings
        setContentPadding(...)

        // Since 4.4.0 Deprecated on 4.5.1 - Adjusts article content top, bottom marigns
        verticalMargin = x to y // Pair of top bottom margin values, in pixels
        
        // Since 4.4.0 - Configures the page's background:
        background = .... // Drawable 
        // e.g. ColorDrawable(Color.BLUE) or ContextCompat.getDrawable(context, R.drawable.bg)

        // Since 4.4.0 - Configure the article's title properties:
        title.apply {
            font = ... // StyleConfig
            // e.g. StyleConfig(16, ContextCompat.getColor(context, R.color.color_def), Typeface.DEFAULT)
            background = ... // Drawable
            // e.g. ColorDrawable(Color.RED) or ContextCompat.getDrawable(context, R.drawable.title_bg)
        }

        // Since 4.4.0 - Configures the article's body properties:
        body.apply {
            // Sets font configurations, to the article content.
            setFont(14.px, Color.RED, "serif", Typeface.BOLD) // DEPRECATED!! on 4.7.0
            // From 4.7.0 use:
            setFont("monospace", 14.px, Color.WHITE, Typeface.ITALIC)
            
            // Since 4.7.0 custom font can be set for the article content as well:
            // setFont("great_vibes", "file:///android_asset/fonts/great_vibes.otf", StyleConfig(...))
            // setFont("bombing", "file:///android_res/font/bombing.ttf", StyleConfig(14.px, Color.RED))
            
            background = ... // Int color
            // e.g. Color.BLACK
        }
    }
}
```
</details>


##### 📚 How to configure font style to article body
{: .notice .mt-6 .mb-4}

Font configurations can be set with `BodyUIConfig.setFont` methods, and will be applied to the article display.   
The article content is displayed in a `WebView` as an HTML page, so font configurations are translated to `CSS` definitions.

> The SDKs font configurations overrides the configurations that were set to the article in the console editor. The changes defined for the mobile display do not change the actual article source.
{: .mb-4}

##### 👉  Configure a built-in font
{: .gray .no_toc}
`setFont(fontFamily, fontSize, fontColor, typefaceStyle)`   
- `fontFamily` is the font name. e.g. monospace, serif, condensed, etc

##### 👉  Configure a custom font
{: .gray .no_toc}
`setFont(fontFamily, fontUrl, styleConfig)`   
Custom fonts setting will take effect on devices with API > 18.   
Custom fonts can be located on `assets` directory or available as `font resource`.   
  
- `fontUrl` - Defines the font location, in url format.   
  - A file Uri points to a font file located on the project.   
    e.g. `file:///android_asset/fonts/myFont.otf`
  - A url link points to a font located remotely (available on API > 22).   
    e.g. `https://....../myfont.woff2`


⚜️ The SDK provides 2 helper methods to create a file uri:
{: .mt-7 .gray}
- `assetsFileUri(assetPath:String)` - Gets the path to the font file in the assets directory.    
  ```kotlin
  // e.g. create file:///android_asset/fonts/myFont.otf
  assetsFileUri("fonts/myFont.otf") 
  ```

- `resourceFileUri(resourcePath:String)` - Gets the path to the resource file in the resources directory   
  ```kotlin
  // e.g. create file:///android_res/font/fontRes.ttf`
  resourceFileUri("font/fontRes.ttf") 
  ```
  > Font resources are supported on API > 25, but can be used as fontUrl, as mentioned above, on devices with lower API level.
  {: .small-remark}

{: .mt-7}
```kotlin
val customProvider = ChatUIProvider(context).apply {
    articleUIConfig.apply {
        // Sets to custom font named "bombing" which is located on font resource directory:
        body.setFont("bombing", resourceFileUri("font/bombing.ttf"))
        
        // Sets to custom font named "great_vibes" which is located on assets directory.
        // The font will be of size 25px, color gray, and will have "italic" style:
        body.setFont("great_vibes", assetsFileUri("fonts/great_vibes.otf"), StyleConfig(25, Color.GRAY,
                                Typeface.defaultFromStyle(Typeface.ITALIC)))

        // Sets to built-in font with size 20px, blue color and bold italic style 
        body.setFont("monospace", 20, Color.BLUE, Typeface.BOLD_ITALIC)
    }
}
```
  
##### 📚 How to configure font style to article **title**
{: .notice .mt-8 .mb-4}
Article title's font is configured by `StyleConfig`, which defines size, color and type.   
Font type is defined by a Typeface value.  
e.g.: 
`Typeface.Default`, `Typeface.SANS_SERIF`   
[`Typeface.defaultFromStyle(Typeface.ITALIC)`](https://developer.android.com/reference/android/graphics/Typeface#defaultFromStyle(int))   
Typeface can also be created from a custom font (which should be located on the app's project)   
[`Typeface.createFromAsset(...)`](https://developer.android.com/reference/android/graphics/Typeface#createFromAsset(android.content.res.AssetManager,%20java.lang.String))   
[`context.resources.getFont(R.font.font_res)`](https://developer.android.com/reference/android/content/res/Resources?hl=nl#getFont(int))

```kotlin
val customProvider = ChatUIProvider(context).apply {
    articleUIConfig.apply {
        // Sets to custom font named "bombing" which is located on font resource directory:
        title.font = StyleConfig(25, Color.GRAY, Typeface.defaultFromStyle(Typeface.ITALIC))

        // Sets to built-in font with size 20px, blue color and bold italic style  
        title.font = StyleConfig(25, Color.GRAY, Typeface.createFromAsset(context.assets, "fonts/great_vibes.otf")
    }
}
```


##### 📚 Article `Close` button
{: .notice .mt-8 .mb-4}
By default a close window button will be available on the page's top right. 
Button configurations are set with `CloseUIConfig`.

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

> Hiding the `Close` window button, can be done by setting it's configuration to null.
  ```kotlin
  val customProvider = ChatUIProvider(context).apply {
      articleUIProvider.articleUIConfig.closeUIConfig = null
  }
  ```

### Channels configurations
{: .mt-8}
The page displayed channels list can be configured using `QuickOptionUIProvider` property.   
Follow [quick options configuration]({{'/docs/chat-configuration-ui-customization/incoming-message/incoming-options#QuickOptions' | relative_url}}), for more details.


