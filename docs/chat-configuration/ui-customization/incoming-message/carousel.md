---
layout: default
title: Carousel
grand_parent: UI Customization
parent: Incoming message 
permalink: /docs/chat-configuration/ui-customization/carousel/
np_toc: true
---

# Carousel {{site.data.vars.need-work}}
{: .no_toc }

1. TOC
{:toc}

---


<img alt='carousel element' src='images/carousel.png' width=50%/>

Displays a "carousel" type response. Used when need to display a list of multiple answers, each has multiple options to select.

The Chat uses a ViewHolder to display the response content in a carousel structure. By default, uses `CarouselChatViewHolder`. The viewHolder can be overridden by providing an implementation of `CarouselViewHolder`, new class or extension to the default. The overriding implementation should be provided on [`ConversationViewsProvider.getCarouselViewHolder`](ConversationViewsProvider#conversationviews).
```java
public CarouselViewHolder getCarouselViewHolder(Context context, 
                                                    CarouselData carouselData){
    return this.carouselViewsProvider.injectCarousel(context, carouselData, null);
}
```

_Carousel consist from 3 sections: <U>Info, Items and Options.</U>_    
   
_Each section subsist of 2 main components: <U>ViewHolder and ViewContainer_ </U>
* **ViewHolder.** Implementation of `com.nanorep.sdkcore.types.ViewHolder` or it's extension `CarouselControlledViewHolder` interface. The view holder should handle the view data updates, and style.
    ```kotlin
    interface ViewHolder {
        fun update(data: Any?) : Any?
        fun getView(): View?
        fun clear() { }
    }
    ```
*  **ViewContainer.** a UI component which will implement a specific extension interface for `ViewContainer`. The interface will define the view requirements for the carousel section.
    ```kotlin
    interface ViewContainer {
        fun getView(): View {
            return this as View
        }
        fun clear(){}
    }
    ```

Carousel section has provided implementations. Each implementation provides a set of configurable attributes.   
Carousel sections can be override by extending `CarouselViewsProvider` class, and provide overriding implementations.
Carousel's views can be customized by either using the provided views and changing them by using the provided attributes setters, defined in the view's ViewContainer interface, or providing a new view that implements that interface. 



## Carousel Info
![Quick Options](images/carousel-info.png)

The textual section, title or some common text displayed over all carousel items. By default this content is being displayed in a bubble above the carousel items list.      

Default Info section implementations are provided by the following: 
```kotlin
open class CarouselViewsProvider {
    ...

    // this method is final. returns the carousel Info section.
    fun injectCarouselInfoArea(context: Context, 
                                carouselData: CarouselData?): ViewHolder? {
        return getCarouselInfoHolder(carouselData, getCarouselInfoView(context))
    }

    /**
     * override this to change info view. provide a new CarouselInfoContainer or
     * apply property changes to provided one
     */
    open fun getCarouselInfoView(context: Context): CarouselInfoContainer {
        return CarouselInfoView(context).apply {
            setIconAlignment(UiConfigurations.Alignment.AlignTop)
        }
    }

    /**
     * override this to change provided info view handler implementation
     */
    open fun getCarouselInfoHolder(carouselData: CarouselData?, carouselInfoContainer: CarouselInfoContainer?): ViewHolder? {
        return CarouselInfoHolder(carouselInfoContainer).update(carouselData)
    }
}
```
Both the ViewHolder and the ViewContainer can be override, by overriding the above methods.
Carousel info section view must implement the `CarouselInfoContainer` interface.
```kotlin
interface CarouselInfoContainer : ViewContainer {

    @Retention(AnnotationRetention.SOURCE)
    @IntDef(Alignment.AlignBottom.toLong(), Alignment.AlignTop.toLong())
    annotation class IconAlignment

    fun setMargins(left:Int, top:Int, right:Int, bottom:Int)
    fun setText(text:Spanned){}
    fun setTime(time:Long){}
    fun enableTimestamp(state:Boolean){}
    fun setIcon(icon:Drawable?){}

    fun setIconAlignment(@IconAlignment alignment: Int){}
    fun setIconTextGap(pixelsGap:Int){}

    fun setTextBackground(background: Drawable?){}
    fun setTextStyle(styleConfig: StyleConfig)
    fun setTimestampStyle(timestampStyle: TimestampStyle)
    fun setDefaultStyle(timestampStyle: TimestampStyle?, styleConfig: StyleConfig?)
}
```

In order to change the default provided look of the info section, the relevant method  `CarouselViewProvided:getCarouselInfoView` should be override. 
#### _Overriding the Carousel info view implementation:_
 - **_Customizing the provided CarouselInfoView._** In the overriding method call for super method to get the default implementation, than change the view look and style by using the provided attributes setters.
    ```java 
    class MyCarouselViewsProvider extends CarouselViewsProvider{
        ...

        @NotNull
        @Override
        public CarouselInfoContainer getCarouselInfoView(@NotNull Context context) {
            CarouselInfoContainer carouselInfoContainer = super.getCarouselInfoView(context);
            
            carouselInfoContainer.setTextBackground(context.getResources()
                                                    .getDrawable(R.drawable.bg_inbox_normal));
            
            // set margins around the info section
            carouselInfoContainer.setMargins( 0, 0, getPx(30),0);

            // if agent icon is available, defines its position relative to bubble
            // available values defined by "CarouselInfoContainer.IconAlignment"
            carouselInfoContainer.setIconAlignment(UiConfigurations.Alignment.AlignTop);
            
            // change the margin between the bot icon and the bubble text
            carouselInfoContainer.setIconTextGap(getPx(4)); 
            
            // change the text style of the carousel info section (in bubble)
            carouselInfoContainer.setTextStyle(new StyleConfig(getPx(18), Color.RED, 
                                                getTypeface(context, "great_vibes.otf"))); 
            
            // change the carousel timestamp display (if not set, will use the style
            // provided by ConversationSettings or default if not available)
            carouselInfoContainer.setTimestampStyle(new TimestampStyle("hh:mm", getPx(11), 
                                                            Color.parseColor("#a8a8a8"), null));

            return carouselInfoContainer;
        }

    }
    ```
- **_Providing a new/extension view that implements the `CarouselInfoContainer` interface._**
    

    ```java
    class MyCarouselViewsProvider extends CarouselViewsProvider{
        ...

        @NotNull
        @Override
        public CarouselInfoContainer getCarouselInfoView(@NotNull Context context) {
            return new OverrideCarouselInfoView(context);
        }
        
    }

    class OverrideCarouselInfoView extends LinearLayout implements CarouselInfoContainer{

        TextView bubbleIncomingText;
        TextView bubbleIncomingTimestamp;

        public OverrideCarouselInfoView(@NotNull Context context) {
            super(context);
            View.inflate(getContext(), R.layout.carousel_info_text, this);
            LinearLayout.LayoutParams params = new LinearLayout.LayoutParams(LayoutParams.WRAP_CONTENT, LayoutParams.WRAP_CONTENT);
            this.setLayoutParams(params);
            this.setPadding(UtilsKt.getPx(4),UtilsKt.getPx(6),0,UtilsKt.getPx(20));
            this.setId(R.id.carousel_info_view);
            this.bubbleIncomingText = (TextView) this.findViewById(R.id.viewholder_remote_bubble_message_textview);
            this.bubbleIncomingTimestamp = (TextView) this.findViewById(R.id.rbs_remote_timestamp);
        }

        //.. implementations for interface:
    }

    -- OR --

    class OverrideCarouselInfoView extends CarouselInfoView {
        ChatTextView chatTextView;

        public OverrideCarouselInfoView(@NotNull Context context) {
            super(context);

            chatTextView = (ChatTextView) findViewById(R.id.cr_info_text);

            ConstraintSet set = new ConstraintSet();
            set.clone(this);
            set.clear(chatTextView.getId(), ConstraintSet.TOP);
            set.connect(chatTextView.getId(), ConstraintSet.BOTTOM, ConstraintSet.PARENT_ID, ConstraintSet.BOTTOM);
            set.applyTo(this);

            ConstraintLayout.LayoutParams params = (ConstraintLayout.LayoutParams) chatTextView.getLayoutParams();
            params.bottomMargin = getPx(5);
        }
    }
    ```


## Carousel Items
<img alt='carousel items' src='images/carousel-items.png' width=40%/>

A list of answers related to the user request. Each item usually consist of title, image and some options for user selection. Items are displayed in a scrollable list, and user can browse between them.      

Default Items section implementations are provided by the following: 
```kotlin
open class CarouselViewsProvider {
    ...

    /**
     * creates the carousel items list area
     */
    fun injectCarouselItemsArea(context: Context, carouselData: CarouselData?): ViewHolder? {
        return getCarouselItemsHolder(carouselData, getCarouselItemsContainer(context, null))
    }

    /**
     * override this to change the provided items view or to change default properties
     */
    open fun getCarouselItemsContainer(context: Context, optionListener: OptionActionListener?): CarouselItemsContainer {
        return CarouselItemsView(context).apply {
            setInfoTextSize(12F)
            setOptionsHorizontalAlignment(Gravity.CENTER)
            setOptionsVerticalAlignment(Gravity.TOP)
            notifyChanges()
        }
    }

    /**
     * override this to change provided items list view handler implementation
     */
    open fun getCarouselItemsHolder(carouselData: CarouselData?, carouselItemsContainer: CarouselItemsContainer?): ViewHolder? {
        return CarouselItemsHolder(carouselItemsContainer).update(carouselData)
    }

}
```
Both the ViewHolder and the ViewContainer can be override, by overriding the above methods.
Carousel *Items* section view must implement the `CarouselItemsContainer` interface.
```kotlin
interface CarouselItemsContainer : OptionsContainer<MultiAnswerItem> {
    fun setOptionsHorizontalAlignment(gravity: Int) {}
    fun setOptionsVerticalAlignment(gravity: Int) {} // options alignment, as one block
    fun setOptionsTextSize(textSize: Float) {}
    fun setOptionsTextStyle(color: Int = -1, fontName: String? = null) {}
    fun setOptionsBackground(background:Drawable?){}
    fun setOptionsLineCount(count:Int){}
    fun overrideOptionsLayoutResource(@LayoutRes optionLayoutRes: Int?) {}
    fun setInfoTextStyle(color: Int = -1, fontName: String? = null) {}
    fun setInfoTextAlignment(@CarouselItemInfoAlignment alignment: Long) {}
    fun setInfoTextBackground(background: Drawable?) {}
    fun setInfoTextSize(textSize: Float) {}
    fun setInfoTitleMinLines(lines: Int) {}
    fun setInfoSubTitleMinLines(lines: Int) {}
    fun setItemBackground(background: Drawable?) {}
    fun setCardStyle(radius:Float = 0f, elevation:Float = 0f){}
    fun setData(dataItems: Array<MultiAnswerItem>?, articleId: Long = 0) {}
}
```

In order to change the default provided UI component for the *Items* section, the relevant method  `CarouselViewProvided:getCarouselItemsContainer` should be override. 
#### _Overriding the Carousel <U>Items</U> view implementation:_
 - **_Customizing the provided CarouselItemsView._** In the overriding method, call the super method to get the default implementation, than change the view look and style by using the provided attributes setters.
    ```java 
    class MyCarouselViewsProvider extends CarouselViewsProvider{
        ...

        @NotNull
        @Override
        public CarouselItemsContainer getCarouselItemsContainer(@NotNull Context context, @Nullable OptionActionListener optionListener) {
            CarouselItemsContainer itemsContainer = super.getCarouselItemsContainer(context, optionListener);

            itemsContainer.setOptionsHorizontalAlignment(Gravity.END);
            itemsContainer.setOptionsVerticalAlignment(Gravity.BOTTOM);
            itemsContainer.setOptionsBackground(getResources().getDrawable(R.drawable.c_item_option_back));
            itemsContainer.setOptionsTextStyle(Color.parseColor("#5EC4B6"), null);
            itemsContainer.setOptionsTextSize(16);
            itemsContainer.setOptionsLineCount(2);

            // background for each item in the list.
            itemsContainer.setItemBackground(getResources().getDrawable( R.drawable.bkg_bots));
            //itemsContainer.setItemBackground(new ColorDrawable(Color.parseColor("#f1f8ff")));
            
            // set the carousel items to have a "card" look. [roundCornersRadius, elevation]
            itemsContainer.setCardStyle(getPx(10), getPx(5) );

            // configure the titles position in the carousel item.
            itemsContainer.setInfoTextAlignment(CarouselItemConfiguration.ItemInfoAlignment.AlignBelowThumb);
            itemsContainer.setInfoTextStyle(Color.GRAY, null);
            itemsContainer.setInfoTextSize(16);
            itemsContainer.setInfoTextBackground(new ColorDrawable(Color.parseColor("#00000000")));

            return itemsContainer;
        }

    }
    ```
- **_Providing a new/extension view that implements the `CarouselItemsContainer` interface._**
    

    ```java
    class MyCarouselViewsProvider extends CarouselViewsProvider{
        ...

        @NotNull
        @Override
        public CarouselItemsContainer getCarouselItemsContainer(@NotNull Context context, @Nullable OptionActionListener optionListener) {
            return new OverrideCarouselItemsView(context, optionListener);
        }
        
    }

    class OverrideCarouselItemsView extends RecyclerView implements CarouselItemsContainer{
        ...
    }

    -- OR --

    class OverrideCarouselItemsView extends CarouselItemsView {
        ...
    }
    ```

* Carousel's item internal options should be configured with the same line count. Defaults to line count = 1.   
In order to change the line count (same for all options) use `CarouselItemsContainer.setOptionsLineCount` method.

---

>__**Notice, If items are configured to have a card look, Items background should not be transparent!**__

---

* Carousel item structure can changed, and Items titles can be positioned in the following locations:
    ```kotlin
    class CarouselItemConfiguration {

        /**
        * defines alignments options for the carousel items textual section, 
        * the titles of each carousel item.
        */
        object ItemInfoAlignment {
            const val AlignThumbTop = 1L
            const val AlignThumbBottom = 2L
            const val AlignBelowThumb = 3L
            const val AlignAboveThumb = 4L
            const val AlignBelowOptions = 5L
        }

    }
    ```
    Using the following statement:
    ```java
    itemsContainer.setInfoTextAlignment(CarouselItemConfiguration.ItemInfoAlignment...);
    ```

* Carousel item Info, title and sub-title, are configured with minimum line number. By default, title configured with 1 line, sub-title configured with 2 lines. In case text takes less than the configured minimum line number it will still size to the configured minimum.    
If possible sub-title text will be displayed to the maximum space available.  
In order to change those values , override the method `getCarouselItemsContainer` in `CarouselViewProvider` and set the custom line number. 
    ```java
    itemsContainer.setInfoTitleMinLines(1); 
    itemsContainer.setInfoSubTitleMinLines(2);
    ```

    <img alt='carousel long sub-title' src='images/carousel-sub-title.png' width=40%/>
 
## Carousel Options

<img alt='carousel options' src='images/carousel-options.png' width=40%/>

An optional list of options displayed to the user, `Quick Options`, usually as a continuance request or navigation action following the last response. Options applies to all carousel items, options of the carousel response itself.
Once an option was selected or a new request sent by the user, those options will disappear. 
Carousels `Quick options` are the same as other response types quick options.     

Default Options section implementations are provided by the following: 
```kotlin
open class CarouselViewsProvider {
    ...

   /**
     * creates the quick options area
     */
    fun injectCarouselOptionsArea(context: Context, carouselData: CarouselData?): ViewHolder? {
        return getCarouselOptionsHolder(carouselData, getCarouselOptionsContainer(context))
    }

    /**
     * override this to change quick options view
     */
    open fun getCarouselOptionsContainer(context: Context): CarouselOptionsContainer {
        return CarouselOptionsView(context)
    }

    /**
     * override this to change provided quick options view holder implementation
     */
    open fun getCarouselOptionsHolder(carouselData: CarouselData?, carouselOptionsContainer: CarouselOptionsContainer): ViewHolder? {
        return CarouselOptionsHolder(carouselOptionsContainer).apply { update(carouselData) }
    }

}
```
Both the ViewHolder and the ViewContainer can be override, by overriding the above methods.
Carousel *Options* section view must implement the `CarouselOptionsContainer` interface.
```kotlin
interface CarouselOptionsContainer : OptionsContainer<QuickOption>{
    fun setOptionsAdapter(adapter: QuickOptionsAdapter){}
}

interface OptionsContainer<T> : ViewContainer {
    fun setItems(dataItems: Array<T>?) {}
    fun setOptionActionListener(listener: OptionActionListener?){}
    fun notifyChanges(){}
}
```
Default implementation for the options section is actually a RecyclerView that inflates views according to a defined layout id. This implementation is not configurable, if a different look or behavior is needed, override the relevant method `CarouselViewsProvider:getCarouselOptionsContainer` and apply a different implementation.



