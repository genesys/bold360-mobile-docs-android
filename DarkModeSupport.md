#  Dark Theme Support

Currently the Android's `Dark Theme` is not being formally supported yet.
The App can only add resources (Drawables, Colors, Styles etc...) in order to override the SDK's resources.

The `Dark Theme` resources needs to be located in additional resources folders as explained below.

## Apply Dark Theme support for your App

You can apply the `Dark Theme` support according to the official android [docs](https://developer.android.com/guide/topics/ui/look-and-feel/darktheme).

## How to customize

- ### Add resource folders

    Add resources folders for the `Dark Theme`.
    Those folders needs to be named as the original resource folders with `-night-` postfix (exp: `drawable-night-hdpi`).

    When using `Dark Theme` the App would automaticaly use those resources.

 - ### Override resources

    The wanted resources to be overriden needs to be located at the previouslly created resources folders abd be named excectelly as the SDK's resources.
    As an example blease follow the below form resources names.
    Please contact us if other resources names are needed.

    <details><summary>Forms Resources names to be overriden</summary>
    <p>

      **The form resources ids are:**

      <details>
      <summary>colors</summary>
      <p>

      * form_field_hint
      * form_field_text
      * form_field_main_text
      * form_field_sub_text
      * form_field_available
      * form_field_not_available
      * form_field_background
      * form_selection_view_first_item_background
      * form_selection_view_background

    </p>
    </details>

    <details><summary>styles</summary>
      <p>

    ```Kotlin
    <style name="MatchSpinnerStyle" parent="android:style/Widget.ListView.DropDown">
            <item name="android:divider">#66aaaaaa</item>
            <item name="android:dividerHeight">1dp</item>
    </style>

    <style name="MatchSpinnerTheme" parent="Theme.AppCompat.Light">
            <item name="android:dropDownListViewStyle">@style/MatchSpinnerStyle</item>
    </style>
    ```

    </p>
    </details>

    <details><summary>drawables</summary>

    <p>

    * form_bg - the form background files name

    </p>

    </details>

      </p>
      </details>