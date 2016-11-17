# Chapter 23: Using the Text Layout Framework {#chapter-23-using-the-text-layout-framework}

Flash Player 10 and later, Adobe AIR 1.5 and later

**Overview of the Text Layout Framework**

**Flash Player 10 and later, Adobe AIR 1.5 and later**

The Text Layout Framework (TLF) is an extensible ActionScript library. The TLF is built on the text engine in Adobe® Flash® Player 10 and Adobe® AIR® 1.5\. The TLF provides advanced typographic and text layout features for innovative typography on the web. The framework can be used with Adobe® Flex® or Adobe® Flash® Professional. Developers can use or extend existing components, or they can use the framework to create their own text components.

The TLF includes the following capabilities:

*   Bidirectional text, vertical text, and over 30 writing scripts including Arabic, Hebrew, Chinese, Japanese, Korean, Thai, Lao, Vietnamese, and others
*   Selection, editing, and flowing text across multiple columns and linked containers
*   Vertical text, Tate-Chu-Yoko (horizontal within vertical text) and justifier for East Asian typography
*   Rich typographical controls, including kerning, ligatures, typographic case, digit case, digit width, and discretionary hyphens
*   Cut, copy, paste, undo, and standard keyboard and mouse gestures for editing
*   Rich developer APIs to manipulate text content, layout, and markup and create custom text components
*   Robust list support including custom markers and numbering formats
*   Inline images and positioning rules

The TLF is an ActionScript 3.0 library built on the Flash Text Engine (FTE) introduced in Flash Player 10\. FTE can be accessed through the flash.text.engine package, which is part of the Flash Player 10 Application Programming Interface (API).

The Flash Player API, however, provides low-level access to the text engine, which means that some tasks can require a relatively large amount of code. The TLF encapsulates the low-level code into simpler APIs. The TLF also provides a conceptual architecture that organizes the basic building blocks defined by FTE into a system that is easier to use.

Unlike FTE, the TLF is not built in to Flash Player. Rather, it is an independent component library written entirely in ActionScript 3.0\. Because the framework is extensible, it can be customized for specific environments. Both Flash Professional and the Flex SDK include components that are based on the TLF framework.

**More Help topics**

[&quot;Flow&quot; TLF markup application](http://sourceforge.net/projects/tlf.adobe/files/current/Flow.swf/download)

**Complex script support**

Flash Player 10 and later, Adobe AIR 1.5 and later

The TLF provides complex script support. Complex script support includes the ability to display and edit right-to-left scripts. The TLF also provides the ability to display and edit a mixture of left-to-right and right-to-left scripts such as Arabic and Hebrew. The framework not only supports vertical text layout for Chinese, Japanese, and Korean, but also supports tate-chu-yoko (TCY elements). TCY elements are blocks of horizontal text embedded into vertical runs of text. The following scripts are supported:

*   Latin (English, Spanish, French, Vietnamese, and so on)
*   Greek, Cyrillic, Armenian, Georgian, and Ethiopic
*   Arabic and Hebrew
*   Han ideographs and Kana (Chinese, Japanese, and Korean) and Hangul Johab (Korean)
*   Thai, Lao, and Khmer
*   Devanagari, Bengali, Gurmukhi, Malayalam, Telugu, Tamil, Gujarati, Oriya, Kannada, and Tibetan
*   Tifinagh, Yi, Cherokee, Canadian Syllabics, Deseret, Shavian, Vai, Tagalog, Hanunoo, Buhid, and Tagbanwa

### Using the Text Layout Framework in Flash Professional and Flex {#using-the-text-layout-framework-in-flash-professional-and-flex}

You can use the TLF classes directly to create custom components in Flash. In addition, Flash Professional CS5 provides a new class, fl.text.TLFTextField, that encapsulates the TLF functionality. Use the TLFTextField class to create text fields in ActionScript that use the advanced text display features of the TLF. Create a TLFTextField object the same way you create a text field with the TextField class. Then, use the textFlow property to assign advanced formatting from the TLF classes.

You can also use Flash Professional to create the TLFTextField instance on the stage using the text tool. Then you can use ActionScript to control the formatting and layout of the text field content using the TLF classes. For more information, see TLFTextField in the ActionScript 3.0 Reference for the Adobe Flash Platform.

If you are working in Flex, use the TLF classes. For more information, see

“Using the Text Layout Framework” on

page 427

.

**Using the Text Layout Framework**

Flash Player 10 and later, Adobe AIR 1.5 and later

If you are working in Flex or are building custom text components, use the TLF classes. The TLF is an ActionScript

3.0 library contained entirely within the textLayout.swc library. The TLF library contains about 100 ActionScript 3.0 classes and interfaces organized into ten packages. These packages are subpackages of the flashx.textLayout package.

**The Text Layout Framework classes**

Flash Player 10 and later, Adobe AIR 1.5 and later

The TLF classes can be grouped into three categories:

*   Data structures and formatting classes
*   Rendering classes
*   User interaction classes

**Data structures and formatting classes**

The following packages contain the data structures and formatting classes for the TLF:

*   flashx.textLayout.elements
*   flashx.textLayout.formats
*   flashx.textLayout.conversion

The main data structure of the TLF is the text flow hierarchy, which is defined in the elements package. Within this structure, you can assign styles and attributes to runs of text with the formats package. You can also control how text is imported to, and exported from, the data structure with the conversion package.

Rendering classes

The following packages contain the rendering classes for the TLF:

*   flashx.textLayout.factory
*   flashx.textLayout.container
*   flashx.textLayout.compose

The classes in these packages facilitate the rendering of text for display by Flash Player. The factory package provides a simple way to display static text. The container package includes classes and interfaces that define display containers for dynamic text. The compose package defines techniques for positioning and displaying dynamic text in containers.

User interaction classes

The following packages contain the user interaction classes for the TLF:

*   flashx.textLayout.edit
*   flashx.textLayout.operations
*   flashx.textLayout.events

The edit and operations packages define classes that you can use to allow editing of text stored in the data structures. The events package contains event handling classes.

### General steps for creating text with the Text Layout Framework {#general-steps-for-creating-text-with-the-text-layout-framework}

The following steps describe the general process for creating text with the Text Layout Format:

1.  Import formatted text into the TLF data structures. For more information, see

    “Structuring text with TLF” on

    page 432

    and

    “Formatting text with TLF” on page 436

    .
2.  Create one or more linked display object containers for the text. For more information, see

    “Managing text

    containers with TLF” on page 437

    .
3.  Associate the text in the data structures with the containers and set editing and scrolling options. For more information, see

    “Enabling text selection, editing, and undo with TLF” on page 438

    .
4.  Create an event handler to reflow the text in response to resize (or other) events. For more information, see

    “Event

    handling with TLF” on page 439

    .

### Text Layout Framework example: News layout {#text-layout-framework-example-news-layout}

Flash Player 10 and later, Adobe AIR 1.5 and later

The following example demonstrates using the TLF to lay out a simple newspaper page. The page includes a large headline, a subhead, and a multicolumn body section:

package

{

import flash.display.Sprite; import flash.display.StageAlign;

import flash.display.StageScaleMode; import flash.events.Event;

import flash.geom.Rectangle;

import flashx.textLayout.compose.StandardFlowComposer; import flashx.textLayout.container.ContainerController; import flashx.textLayout.container.ScrollPolicy;

import flashx.textLayout.conversion.TextConverter; import flashx.textLayout.elements.TextFlow;

import flashx.textLayout.formats.TextLayoutFormat;

public class TLFNewsLayout extends Sprite

{

private var hTextFlow:TextFlow; private var headContainer:Sprite;

private var headlineController:ContainerController; private var hContainerFormat:TextLayoutFormat;

private var bTextFlow:TextFlow; private var bodyTextContainer:Sprite;

private var bodyController:ContainerController; private var bodyTextContainerFormat:TextLayoutFormat;

private const headlineMarkup:String = &quot;&lt;flow:TextFlow [xmlns:flow=&#039;http://ns.adobe.com/textLayout/2008&#039;&gt;&lt;flow:p](http://ns.adobe.com/textLayout/2008%27) textAlign=&#039;center&#039;&gt;&lt;flow:span fontFamily=&#039;Helvetica&#039; fontSize=&#039;18&#039;&gt;TLF News Layout Example&lt;/flow:span&gt;&lt;flow:br/&gt;&lt;flow:span fontFamily=&#039;Helvetica&#039; fontSize=&#039;14&#039;&gt;This example formats text like a newspaper page with a headline, a subtitle, and multiple columns&lt;/flow:span&gt;&lt;/flow:p&gt;&lt;/flow:TextFlow&gt;&quot;;

private const bodyMarkup:String = &quot;&lt;flow:TextFlow [xmlns:flow=&#039;http://ns.adobe.com/textLayout/2008&#039;](http://ns.adobe.com/textLayout/2008%27) fontSize=&#039;12&#039; textIndent=&#039;10&#039; marginBottom=&#039;15&#039; paddingTop=&#039;4&#039; paddingLeft=&#039;4&#039;&gt;&lt;flow:p marginBottom=&#039;inherit&#039;&gt;&lt;flow:span&gt;There are many

&lt;/flow:span&gt;&lt;flow:span fontStyle=&#039;italic&#039;&gt;such&lt;/flow:span&gt;&lt;flow:span&gt; lime-kilns in that tract of country, for the purpose of burning the white marble which composes a large part of the substance of the hills. Some of them, built years ago, and long deserted, with weeds growing in the vacant round of the interior, which is open to the sky, and grass and wild-flowers rooting themselves into the chinks of the stones, look already like relics of antiquity, and may yet be overspread with the lichens of centuries to come. Others, where the lime-burner still feeds his daily and nightlong fire, afford points of interest to the wanderer among the hills, who seats himself on a log of wood or a fragment of marble, to hold a chat with the solitary man. It is a lonesome, and, when the character is inclined to thought, may be an intensely thoughtful occupation; as it proved in the case of Ethan Brand, who had mused to such strange purpose, in days gone by, while the fire in this very kiln was burning.&lt;/flow:span&gt;&lt;/flow:p&gt;&lt;flow:p marginBottom=&#039;inherit&#039;&gt;&lt;flow:span&gt;The man who now watched the fire was of a different order, and troubled himself with no thoughts save the very few that were requisite to his business. At frequent intervals, he flung back the clashing weight of the iron door, and, turning his face

from the insufferable glare, thrust in huge logs of oak, or stirred the immense brands with a long pole. Within the furnace were seen the curling and riotous flames, and the burning marble, almost molten with the intensity of heat; while without, the reflection of the fire quivered on the dark intricacy of the surrounding forest, and showed in the foreground a bright and ruddy little picture of the hut, the spring beside its door, the athletic and coal-begrimed figure of the lime-burner, and the half-frightened child, shrinking into the protection of his father&#039;s shadow. And when again the iron door was closed, then reappeared the tender light of the half- full moon, which vainly strove to trace out the indistinct shapes of the neighboring mountains; and, in the upper sky, there was a flitting congregation of clouds, still faintly tinged with the rosy sunset, though thus far down into the valley the sunshine had vanished long and long ago.&lt;/flow:span&gt;&lt;/flow:p&gt;&lt;/flow:TextFlow&gt;&quot;;

public function TLFNewsLayout()

{

//wait for stage to exist addEventListener(Event.ADDED_TO_STAGE, onAddedToStage);

}

private function onAddedToStage(evtObj:Event):void

{

removeEventListener(Event.ADDED_TO_STAGE, onAddedToStage); stage.scaleMode = StageScaleMode.NO_SCALE;

stage.align = StageAlign.TOP_LEFT;

// Headline text flow and flow composer

hTextFlow = TextConverter.importToFlow(headlineMarkup, TextConverter.TEXT_LAYOUT_FORMAT);

// initialize the headline container and controller objects headContainer = new Sprite();

headlineController = new ContainerController(headContainer); headlineController.verticalScrollPolicy = ScrollPolicy.OFF; hContainerFormat = new TextLayoutFormat(); hContainerFormat.paddingTop = 4;

hContainerFormat.paddingRight = 4;

hContainerFormat.paddingBottom = 4;

hContainerFormat.paddingLeft = 4;

headlineController.format = hContainerFormat; hTextFlow.flowComposer.addController(headlineController); addChild(headContainer); stage.addEventListener(flash.events.Event.RESIZE, resizeHandler);

// Body text TextFlow and flow composer

bTextFlow = TextConverter.importToFlow(bodyMarkup, TextConverter.TEXT_LAYOUT_FORMAT);

// The body text container is below, and has three columns bodyTextContainer = new Sprite();

bodyController = new ContainerController(bodyTextContainer); bodyTextContainerFormat = new TextLayoutFormat(); bodyTextContainerFormat.columnCount = 3;

bodyTextContainerFormat.columnGap = 30;

bodyController.format = bodyTextContainerFormat; bTextFlow.flowComposer.addController(bodyController); addChild(bodyTextContainer);

resizeHandler(null);

}

private function resizeHandler(event:Event):void

{

const verticalGap:Number = 25; const stagePadding:Number = 16;

var stageWidth:Number = stage.stageWidth - stagePadding; var stageHeight:Number = stage.stageHeight - stagePadding; var headlineWidth:Number = stageWidth;

var headlineContainerHeight:Number = stageHeight;

// Initial compose to get height of headline after resize headlineController.setCompositionSize(headlineWidth,

headlineContainerHeight);

hTextFlow.flowComposer.compose();

var rect:Rectangle = headlineController.getContentBounds(); headlineContainerHeight = rect.height;

// Resize and place headline text container

// Call setCompositionSize() again with updated headline height headlineController.setCompositionSize(headlineWidth, headlineContainerHeight ); headlineController.container.x = stagePadding / 2; headlineController.container.y = stagePadding / 2; hTextFlow.flowComposer.updateAllControllers();

// Resize and place body text container

var bodyContainerHeight:Number = (stageHeight - verticalGap - headlineContainerHeight);

bodyController.format = bodyTextContainerFormat; bodyController.setCompositionSize(stageWidth, bodyContainerHeight ); bodyController.container.x = (stagePadding/2);

bodyController.container.y = (stagePadding/2) + headlineContainerHeight +

verticalGap;

}

}

}

bTextFlow.flowComposer.updateAllControllers();

The TLFNewsLayout class uses two text containers. One container displays a headline and subhead, and the other displays three-column body text. For simplicity, the text is hard-coded into the example as TLF Markup text. The headlineMarkup variable contains both the headline and the subhead, and the bodyMarkup variable contains the body text. For more information on TLF Markup, see

“Structuring text with TLF” on page 432

.

After some initialization, the onAddedToStage() function imports the headline text into a TextFlow object, which is the main data structure of the TLF:

hTextFlow = TextConverter.importToFlow(headlineMarkup, TextConverter.TEXT_LAYOUT_FORMAT);

Next, a Sprite object is created for the container, and a controller is created and associated with the container:

headContainer = new Sprite();

headlineController = new ContainerController(headContainer);

The controller is initialized to set formatting, scrolling, and other options. The controller contains geometry that defines the bounds of the container that the text flows into. A TextLayoutFormat object contains the formatting options:

hContainerFormat = new TextLayoutFormat();

The controller is assigned to the flow composer and the function adds the container to the display list. The actual composition and display of the containers is deferred to the resizeHandler() method. The same sequence of steps is performed to initialize the body TextFlow object.

The resizeHandler() method measures the space available for rendering the containers and sizes the containers accordingly. An initial call to the compose() method allows for the calculation of the proper height of the headline container. The resizeHandler() method can then place and display the headline container with the updateAllControllers() method. Finally, the resizeHandler() method uses the size of the headline container to determine the placement of the body text container.