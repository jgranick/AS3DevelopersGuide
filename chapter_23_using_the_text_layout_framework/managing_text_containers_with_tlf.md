## Managing text containers with TLF {#managing-text-containers-with-tlf}

Flash Player 10 and later, Adobe AIR 1.5 and later

Once text is stored in the TLF data structures, Flash Player can display it. The text that is stored in the flow hierarchy must be converted into a format that Flash Player can display. The TLF offers two ways to create display objects from a flow. The first, more simple approach is suitable for displaying static text. The second, more complicated approach lets you create dynamic text that can be selected and edited. In both cases, the text is ultimately converted into instances of the TextLine class, which is part of the flash.text.engine.* package in Flash Player 10.

**Creating static text**

The simple approach uses the TextFlowTextLineFactory class, which can be found in the flashx.textLayout.factory package. The advantage of this approach, beyond its simplicity, is that it has a smaller memory footprint than does the FlowComposer approach. This approach is advisable for static text that the user does not need to edit, select, or scroll.

For more information, see TextFlowTextLineFactory in the ActionScript 3.0 Reference for the Adobe Flash Platform.

### Creating dynamic text and containers {#creating-dynamic-text-and-containers}

Use a flow composer if you want to have more control over the display of text than that provided by TextFlowTextLineFactory. For example, with a flow composer, your users can select and edit the text. For more information, see

“Enabling text selection, editing, and undo with TLF” on page 438

.

A flow composer is an instance of the StandardFlowComposer class in the flashx.textLayout.compose package. A flow composer manages the conversion of TextFlow into TextLine instances, and also the placement of those TextLine instances into one or more containers.

TextFlow

TextLine

TextLine

_An IFlowComposer has zero or more ContainerControllers_

Every TextFlow instance has a corresponding object that implements the IFlowComposer interface. This IFlowComposer object is accessible through the TextFlow.flowComposer property. You can call methods defined by the IFlowComposer interface through this property. These methods allow you to associate the text with one or more containers and prepare the text for display within a container.

A container is an instance of the Sprite class, which is a subclass of the DisplayObjectContainer class. Both of these classes are part of the Flash Player display list API. A container is a more advanced form of the bounding rectangle used in with TextLineFactory class. Like the bounding rectangle, a container defines the area where TextLine instances appear. Unlike a bounding rectangle, a container has a corresponding “controller” object. The controller manages scrolling, composition, linking, formatting, and event handling for a container or set of containers. Each container has a corresponding controller object that is an instance of the ContainerController class in the flashx.textLayout.container package.

To display text, create a controller object to manage the container and associate it with the flow composer. Once you have the container associated, compose the text so that it can be displayed. Accordingly, containers have two states: composition and display. Composition is the process of converting the text from the text flow hierarchy into TextLine instances and calculating whether those instances fit into the container. Display is the process of updating the Flash Player display list.

For more information, see IFlowComposer, StandardFlowComposer, and ContainerController in the ActionScript 3.0 Reference for the Adobe Flash Platform.