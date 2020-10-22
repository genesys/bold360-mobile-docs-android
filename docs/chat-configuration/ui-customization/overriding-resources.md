---
layout: default
title: Overriding Resources
parent: UI Customization
grand_parent: Chat Configuration 
permalink: /docs/chat-configuration/ui-customization/overriding-resources
nav_order: 9
---

# Overriding Resources {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
SDK resources can be overridden by the hosting app, by defining the same resource
{: .overview}

Here you can see the supported SDK resources to be overidden.
> Note: This list can be changed from time to time together with [customizations](/docs/chat-configuration/ui-customization/how-it-works) development

    
<details><summary>Colors</summary>
    
  - Live Forms
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

<details><summary>Styles</summary>

- Live Forms
  - FormHintTextAppearance - override in order to change the form field hint appearance
  - MatchSpinnerStyle
  - MatchSpinnerTheme

</details>

<details><summary>Drawables</summary>

- Live Forms
  - form_bg
  - submit_button_selector
  - submit_button_idle - The Idle submit button's background shape
  - main_button_pressed - The Pressed submit button's background shape

</details>

<details><summary>Dimens</summary>

- Live Forms
  - form_main_text_style
  - form_sub_text_style
  - form_option_item_padding
  - form_field_padding
  - form_fields_gap

</details>

---

## Related Topics
- [Dark Mode](/docs/chat-configuration/ui-customization/dark-mode)
