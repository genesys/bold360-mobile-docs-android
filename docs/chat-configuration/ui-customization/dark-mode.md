---
layout: default
title: Dark Mode
parent: UI Customization
grand_parent: Chat Configuration 
# permalink: /docs/chat-configuration/ui-customization/dark-mode
nav_order: 11
---

# Dark mode Support 

_Currently, **dark mode** is not formally being supported by the SDK._

### How to apply Dark mode to your Chat
In order to apply dark theme to your chat, you can define overriding and dark mode resources additions, that will replace the resources used by the SDK.

- #### Adding night mode resources
  Night mode resources should be located in a night mode dedicated folder.
  The night mode folder is named as the original resource folder with "-night-"
  addition to it. (e.g., `drawable-night-hdpi`).

  When setting the device to `Night mode`, those resources will be used automatically.

 - #### Overriding resources

    SDK resources can be overridden by the hosting app, by defining the same resource id (name) with different value on the Apps resources.
    
    
  > _More info can be found on the official android developers guide on_ [Dark theme](https://developer.android.com/guide/topics/ui/look-and-feel/darktheme).
  
  ---
  

  <details><summary>Chat forms partial resources list</summary>
    
  - <details><summary>colors</summary>
      
      * form_field_hint
      * form_field_text
      * form_field_main_text
      * form_field_sub_text
      * form_field_available
      * form_field_unavailable
      * form_field_background
      * form_rating_field_background
      * form_selection_dropdown_background
      * form_selection_dropdown_title_background
    </details>

  - <details><summary>styles</summary>

    * FormHintTextAppearance - override in order to change the form field hint appearance
    * MatchSpinnerStyle
    * MatchSpinnerTheme
    </details>

  - <details><summary>drawables</summary>

    * form_bg
    </details>

  - <details><summary>dimens</summary>

    * form_main_text_style
    * form_sub_text_style
    * form_option_item_padding
    * form_field_padding
    * form_fields_gap
    </details>

 </details>