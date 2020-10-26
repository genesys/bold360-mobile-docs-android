---
layout: default
title: Overriding Resources
parent: UI Customization
grand_parent: Chat Configuration 
# permalink: /docs/chat-configuration/ui-customization/overriding-resources
nav_order: 10
---

# Overriding Resources {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
SDK resources can be overridden by the hosting app, by defining a resource with the same id on the hosting Apps resources.   
{: .overview}

> Basically all SDK resources can be overridden, but <u>we don't encurage doing so</u>.   
UI component changes better be applied using [`ChatUIProvider`]({{'/docs/chat-configuration/ui-customization/how-it-works | relative_url}}).
{: .text-purple-000}  

---

## The SDKs resources
Following are some of the SDKs resources that can be overidden.

> _Resources list may be changed from time to time._

{: .mt-6}
<details close markdown="block">
<summary>Colors</summary>
    
  - Live Forms related:

    - form_field_hint
    - form_field_text
    - form_field_main_text
    - form_field_sub_text
    - form_field_available
    - form_field_unavailable
    - form_field_background
    - form_rating_field_background
    - form_selection_dropdown_background
    - form_selection_dropdown_title_background
    - submit_idle - The submit button's idle color
    - submit_pressed - The submit button's pressed color

</details>
{: .mb-3}
<details close markdown="block">
<summary>Styles</summary>

- Live Forms related:

  - FormHintTextAppearance - override in order to change the form field hint appearance
  - MatchSpinnerStyle
  - MatchSpinnerTheme

</details>
{: .mb-3}
<details close markdown="block">
<summary>Drawables</summary>

- Live Forms related:

  - form_bg
  - submit_button_selector
  - submit_button_idle - The Idle submit button's background shape
  - main_button_pressed - The Pressed submit button's background shape

</details>
{: .mb-3}
<details close markdown="block">
<summary>Dimens</summary>

- Live Forms related:

  - form_main_text_style
  - form_sub_text_style
  - form_option_item_padding
  - form_field_padding
  - form_fields_gap

</details>

---

## Related Topics
- [Dark Mode](/docs/chat-configuration/ui-customization/dark-mode)
