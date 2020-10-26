---
layout: default
title: Carousel 
np_toc: true
nav_exclude: true
---

# Carousel 
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc .mb-0}
- [Create a carousel response](https://support.bold360.com/bold360/help/how-to-create-an-image-gallery-in-conversational-articles)

---

## Overview
Displays a "carousel" typed incoming response. 
As a regular incoming message, carousel has a message section with a timestamp and avatar, and the quick options and channels at the bottom. The carousel provides the ability to display multiple related answers to a user query in a horizontal scrollable structure.
{: .overview}
![]({{'/assets/images/carousel.png' | relative_url}})
{: .image-40}


## Carousel Item
Each item in the carousel consists of 3 sections: 
- **Image** - If not configured, a placeholder default image will be displayed.
    Can be configured with a url link. <sub>Optional</sub>.
- **Info** - Title <sub>Mandatory</sub> and a subtitle <sub>Optional</sub> 
- **Options** - Item options <sub>Optional</sub>.

---

## UI configurations
Carousel UI can be configured, by overriding `CarouselItemsUIProvider.configure` method and/or `CarouselItemsUIProvider.customize` method for [dynamic data related customizations]({{'/docs/chat-configuration/ui-cutomization/how-it-works#customize' | relative_url}}).   
Available configuration proprties can be viewed on `CarouselItemsUIAdapter`

### General item configurations
- Use `CarouselItemsUIAdapter.setCardStyle` method to configure the carousel item look. 
  > The carousel items display can be configured to have a card like look which has elevation or be flat, with or without rounded corners. 

  >_**Notice**, If a card style display is configured, items background should not be transparent!_

- Carousel items sections (info, image and options), display order, can be configured with `CarouselItemsUIAdapter.setInfoTextAlignment`, defaults to `CarouselItemConfiguration.ItemInfoAlignment.AlignBelowThumb` 

  ![]({{'/assets/images/customed-carousel.png' | relative_url}})
  {: .image-40}


### Info section
Info section contains a title and a sub-title, which are configured with minimum line number of 1 and 2 lines respectfully.   
Minimun height for this section will be calulated according to those values.
The sub-title height is flexible and may stretch to the maximum available item height.

Minimum line numbers can be configured with `setInfoTitleMinLines` and `setInfoSubTitleMinLines` methods.

![]({{'/assets/images/carousel-info.png' | relative_url}})
{: .image-40}

> Checkout other configurations for the info section on `CarouselItemsUIAdapter` prefixed with **setInfo...**.

### Options section
Item options are contained in a non-scrollable vertical order layout.  
The options, text alignment and look are configurable.

All options are configured to the same line count, defaults to 1 line.   
Use `CarouselItemsContainer.setOptionsLineCount` method to change the line count.
{: .light-title}   
> Carousel view height is being calculated according to the item with the maximum number of options.

### Image section
Image size is constant. 
If a link is configured for the image, it will be passed to the hosting App, on user press action, for further handling, via [`ChatEventListener.onUrlLinkSelected` implementation]({{'/docs/chat-configuration/tracking-events/events-and-notifications#listening-to-url-navigation' | relative_url}}).

---

## UI override
Carousel list section can be override by overriding `CarouselItemsUIProvider.overrideFactory` property.    
A customed implementation of `CarouselItemsAdapter` should be provided by the overriding factory.

```kotlin
ChatUIProvider(context).apply {
    this.chatElementsUIProvider.incomingUIProvider.carouselUIProvider.overrideFactory =
            object : CarouselItemsUIProvider.CarouselFactory {
                override fun createItemsAdapter(context: Context): CarouselItemsAdapter {
                    return MyCarouselImplementation();
                }
    }
}
```     
{: .mb-10}       
