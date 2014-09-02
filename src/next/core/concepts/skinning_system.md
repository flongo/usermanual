Title: AT skinning system



The visual style of an application using Aria Templates is highly configurable through the skinning system.

In AT widgets are grouped under widget libraries and there are four kind of widget libraries i.e Aria, HTML, Embed & Touch widget libraries.
Only the widgets under Aria widget library are skinnable and if we would like to use them in an application it is mandatory to have skin.
Every single widget can have many different visual appearances depending on the skin of the application.
Moreover, even in the same application skin, several different visual appearances of a widget can be configured, to be used in different places in the application.

Skin is nothing but an AT singleton class with classpath as "aria.widgets.AriaSkin" and to use the skin in an application we need load it right after the AT framework is loaded.
Presently, AT provides two skins by default i.e atskin.js and atflatskin.js. User can change the styling of widgets by changing the properties in these skin files or they can
create their own skin file with desired properties. Apart from the styling of widgets, skin also has application specific styling i.e font for all widgets, external CSS to load
with skin, color settings for application etc. For more information, refer [application styling](http://www.ariatemplates.com/aria/guide/apps/apidocs/#aria.widgets.AriaSkinBeans:PageGeneralCfg)

In Aria Templates, a visual appearance of a widget is called a "skinclass". The skinclass to be used in a specific instance of a widget can be defined in the sclass property
 of the widget configuration. By default, the "std" skinclass is used.
For example, here is the same button using two different skinclasses:
<iframe class='samples' src='%SNIPPETS_SERVER_URL%/samples/github.com/ariatemplates/documentation-code/samples/widgets/button/skinning/' ></iframe>

Here is an extract of the template source which produced these buttons:
<script src='%SNIPPETS_SERVER_URL%/snippets/github.com/ariatemplates/documentation-code/snippets/core/skinning/Snippet.tpl?tag=wgtButtonSkin&lang=at&outdent=true'></script>

The first button uses the "std" default skinclass of the button whereas second one uses "important" skinclass of the button. Each skin defines the set of available skinclasses for each skinnable widget. The "std" skinclass must always be defined for every skinnable widget.


## How Skinning works?

All the properties which we use in atskin.js or atflatskin.js for the widgets are documented in [AriaSkinBeans.js](http://www.ariatemplates.com/aria/guide/apps/apidocs/#aria.widgets.AriaSkinBeans).
For example say for Button widget, we have the following properties (**states**, **frame**, **simpleHTML**, **label**) defined in AriaSkinBean.js and also we can see that these properties can inherit from other parent properties through $type (for more info on beans and beans inheritance refer http://ariatemplates.com/usermanual/latest/json_bean_definitions).

<script src='%SNIPPETS_SERVER_URL%/snippets/github.com/ariatemplates/documentation-code/snippets/core/skinning/AriaSkinBeanButton.js?noheader=true&tag=ButtonCfg&lang=javascript&outdent=true' defer></script>
Once the properties are defined in AriaSkinBeans.js, we can use them in atskin.js or atflatskin.js for any skinclass like below. This is the extract of Button configuration from atskin.js.
In this article, we often refer this to explain different concepts of skinning .

<script src='%SNIPPETS_SERVER_URL%/snippets/github.com/ariatemplates/documentation-code/snippets/core/skinning/AtSkinButton.js?tag=ButtonSkin&lang=javascript&outdent=true' defer></script>

From the above json, we can find that there are three skinclasses **important**, **simple**, **std** & for the default skinclass i.e std we can find that we have given the values for the properties defined in AriaSkinBeans.js and these values are used in widgetstyle.tpl.css to generate css for the widget


## Skinclass inheritance

For widget properties, to avoid having to define all properties each time a new skinclass is defined in atskin.js or atflatskin.js, any skinclass inherits from the "std" skinclass (of the same skinnable class), so that if a property is not defined in the skinclass, it will be looked for in the "std" skinclass.
For example for the same Button widget above we can see that for the skinclass "important" only the font weight is given as bold and the other properties are inherited from "std" skinclass.

## States and states inheritance

State is nothing but a state in which a widget can be. As seen before, some skinnable classes, such as the button, define different states (like normal, disabled, msdown ...).
An example of button in a disabled state is as follows:

<iframe class='samples' src='%SNIPPETS_SERVER_URL%/samples/github.com/ariatemplates/documentation-code/samples/widgets/button/skinning/disabled/' ></iframe>

You can note that from the above  that the Button configuration in atskin.js for the disabled state have two properties **color (#B0B0B0)** and **sprIdx (2)**, that is the reason why text color of
above disabled button is **#B0B0B0** and **sprIdx" is the icon index in the sprite image which will be discussed later in this article under Icons section

To avoid having to define all the properties for every state, even when they do not change from state to state, every state automatically inherits from the **normal** state,
so that if a property is not defined for a specific state, it is looked for in the **normal** state.
skinclass inheritance takes precedence over state inheritance.

## Frames

In AT widgets styling will be generated based on Frames. There are four types of frames:

* FixedHeight - Used for widgets which have a fixed height
* Simple - Used for widgets which have a basic frame around them
* Table - Used for widgets which have a layout based on a table
* SimpleHTML - Used for widgets which have no frame at all

For the same Button example above we can see that its frame is "FixedHeight" for "std" skinclass. Depending on the frame type the properties in the widget states will be changed.
For example, for the FixedHeight frame the properties which are used in widget state are documented in AriaSkinBean.js as below

<script src='%SNIPPETS_SERVER_URL%/snippets/github.com/ariatemplates/documentation-code/snippets/core/skinning/AriaSkinBeanButton.js?noheader=true&tag=FixedHeigtFrameStateCfg&lang=javascript&outdent=true' defer></script>

## Icons

In AT, widgets like [DatePicker](http://snippets.ariatemplates.com/samples/github.com/ariatemplates/documentation-code/tree/master/samples/widgets/datepicker/reference),
 [MultiSelect](http://snippets.ariatemplates.com/samples/github.com/ariatemplates/documentation-code/tree/master/samples/widgets/multiselect), [Checkbox](http://snippets.ariatemplates.com/samples/github.com/ariatemplates/documentation-code/tree/master/samples/widgets/checkbox/binding) etc where we can find the icons in them. In this section we will see how these icons are defined in atskin.js and
how we can use them in the widgets to display them.

In atskin.js we have Icon configuration where we can give properties like name, width, height etc to a sprite image. Lets see how we have defined **dropdown** icon under Icon
configuration.
<script src='%SNIPPETS_SERVER_URL%/snippets/github.com/ariatemplates/documentation-code/snippets/core/skinning/AtSkinIcon.js?tag=DropdownIcon&lang=javascript&outdent=true' defer></script>

The sprite image for the above dropdown looks like below:
<br/>
<img src='https://raw.githubusercontent.com/ariatemplates/ariatemplates/master/src/aria/css/atskin/imgs/dropdownbtns.gif' />
<br/>
Lets see the icon properties and its description
* **`content`** which is a json object where keys can be any names and values correspond to index of an icon in the sprite
* **`spriteSpacing`** space between icons in pixels
* **`direction`** either horizontal(x) or vertical(y)
* **`iconWidth`** width of the icon
* **`spriteURL`** url of sprite image
* **`iconHeight`** height of the icon
* **`biDimensional`** set to true when the sprite has rows and columns more than 1


To get the icon for the datepicker widget from the above dropdown icon we define in atskin.js as below:
<script src='%SNIPPETS_SERVER_URL%/snippets/github.com/ariatemplates/documentation-code/snippets/core/skinning/AtSkinIcon.js?tag=DatePickerIcon&lang=javascript&outdent=true' defer></script>


## How to write my own skin?

To write your own custom skin create an AT singleton class with "aria.widgets.AriaSkin".
This is a sample custom skin for the widget **myWidget** with skinclasses **mySkinClass** and **std** (default one)

<script src='%SNIPPETS_SERVER_URL%/snippets/github.com/ariatemplates/documentation-code/snippets/core/skinning/AtSkinGeneric.js?noheader=true&tag=GenericWidgetSkin&lang=javascript&outdent=true' defer></script>

and in tpl user can use the **mySkinClass** for **myWidget** as below
<script src='%SNIPPETS_SERVER_URL%/snippets/github.com/ariatemplates/documentation-code/snippets/core/skinning/Snippet.tpl?tag=wgtMyWidgetSkin&lang=at&outdent=true'></script>