---
layout: default
title: Video Messages 
parent: FAQ
nav_order: 5
permalink: /docs/faq/video-messages
---

# Video Messages
{: no_toc}

---

## Overview
Chat messages displays video indication over messages with video content.    
In order to display the video indication over AI articles, the article should be configured in a way the SDK will recognize the video content it contains.    
This guide will help you configure your video content in a Bot article. 
> Currently we support only `Youtube` videos. 
{: .overview}

---

## Adding youtube video to an article

1. On the bold360ai console open the article you want to update

2. Open the video content that you want to add on youtube.

3. Select `"Share" -> "Embed"` then `"Copy`"

4. Update the copied link so it will not contain the `allowfullscreen` property in the `iFrame` tag or assign it to a value.

    Copied embedded video tag:
    {: .light-title}
    ```xml
    <iframe width="560" height="315" src="https://www.youtube.com/embed/veuyO0Mf3EQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    ```

    Updated embedded video tag:
    {: .light-title}
    ```xml
    <iframe width="560" height="315" src="https://www.youtube.com/embed/veuyO0Mf3EQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"></iframe>
    ```
    or:
    ```xml
    <iframe width="560" height="315" src="https://www.youtube.com/embed/veuyO0Mf3EQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen=""></iframe>
    ```

6. Copy the updated youtube video tag to your article on the bold360ai console and save it.


{: .mt-8}
Article source on the bold360ai console:
![]({{'/assets/images/video-article-console.png' | relative_url}})
{: .image-70}

{: .mt-6}
<details close markdown="block"> 
    
<summary>Article source with video content:</summary>
    
```html
Article source example:
<h2><img alt="" src="https://mir-s3-cdn-cf.behance.net/project_modules/disp/98f67512129037.56d6fccde4698.png" style="width: 400px; height: 400px;" /></h2>
<h2><span style="color:#ff0000;">This is a test for videos from youtube</span></h2>
<p><span style="color:#0000ff;">A.I. and the Next Evolution of Customer Service:</span></p>
<p><iframe src="https://www.youtube.com/embed/iW7KFeNcYBg"></iframe></p>
<p><span style="color:#ff8c00;">The Power of Harmony Between Bots and Humans:</span></p>
<p><iframe src="https://www.youtube.com/embed/qzfNlcxKgHM"></iframe></p>
<p>Bold360: Smart Engagement for Digital World</p>
<p><iframe src="https://www.youtube.com/embed/a7qvOOIGj-o"></iframe></p>
```
</details>


> ⚜️ The SDK doesn't support video playing. The youtube link is sent to the app as event to `ChatEventListener.onUrlLinkSelected` implementation. The hosting app should decide what should be done with the retrieved link.
{: .mt-8}

  ```kotlin
  fun onUrlLinkSelected(url: String) {
      // sample code for handling given link
      try {
          if (isFileUrl(url)) {
              openFileInDefaultApp(url)
          } else {
              val intent = Intent(Intent.ACTION_VIEW).setData(Uri.parse(url))
              this.startActivity(intent)
          }
      } catch (e: Exception) {
          ...
      }
  }
  ```