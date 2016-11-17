# Chapter 35: Drag and drop in AIR {#chapter-35-drag-and-drop-in-air}

Adobe AIR 1.0 and later

Use the classes in the Adobe® AIR™ drag-and-drop API to support user-interface drag-and-drop gestures. A _gesture_ in this sense is an action by the user, mediated by both the operating system and your application, expressing an intent to copy, move, or link information. A _drag-out_ gesture occurs when the user drags an object out of a component or application. A _drag-in_ gesture occurs when the user drags in an object from outside a component or application.

With the drag-and-drop API, you can allow a user to drag data between applications and between components within an application. Supported transfer formats include:

• Bitmaps

• Files

• HTML-formatted text

• Text

• Rich Text Format data

• URLs

• File promises

• Serialized objects

• Object references (only valid within the originating application)

**Basics of drag and drop in AIR**

Adobe AIR 1.0 and later

For a quick explanation and code examples of using drag and drop in an AIR application, see the following quick start articles on the Adobe Developer Connection:

• [Supporting drag-and-drop and copy-and-paste](http://www.adobe.com/devnet/air/flex/quickstart/scrappy_copy_paste.html) (Flex)

• [Supporting drag-and-drop and copy-and-paste](http://www.adobe.com/devnet/air/flash/quickstart/scrappy_copy_paste.html) (Flash) The drag-and-drop API contains the following classes.

| **Package** | **Classes** |
| --- | --- |
| flash.desktop | Constants used with the drag-and-drop API are defined in the following classes: |
| flash.events | [NativeDragEvent](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/events/NativeDragEvent.html) |

Drag-and-drop gesture stages

The drag-and-drop gesture has three stages:

**Initiation** _A user initiates a drag-and-drop operation by dragging from a component, or an item in a component, while holding down the mouse button._ The component that is the source of the dragged item is typically designated as the drag initiator and dispatches nativeDragStart and nativeDragComplete events. An Adobe AIR application starts a drag operation by calling the NativeDragManager.doDrag() method in response to a mouseDown or mouseMove event.

If the drag operation is initiated from outside an AIR application, there is no initiator object to dispatch

nativeDragStart or nativeDragComplete events.

**Dragging** _While holding down the mouse button, the user moves the mouse cursor to another component, application, or to the desktop._ As long as the drag is underway, the initiator object dispatches nativeDragUpdate events. (However, this event is not dispatched in AIR for Linux.) When the user moves the mouse over a possible drop target in an AIR application, the drop target dispatches a nativeDragEnter event. The event handler can inspect the event object to determine whether the dragged data is available in a format that the target accepts and, if so, let the user drop the data onto it by calling the NativeDragManager.acceptDragDrop() method.

As long as the drag gesture remains over an interactive object, that object dispatches nativeDragOver events. When the drag gesture leaves the interactive object, it dispatches a nativeDragExit event.

**Drop** _The user releases the mouse over an eligible drop target._ If the target is an AIR application or component, then the target object dispatches a nativeDragDrop event. The event handler can access the transferred data from the event object. If the target is outside AIR, the operating system or another application handles the drop. In both cases, the initiating object dispatches a nativeDragComplete event (if the drag started from within AIR).

The NativeDragManager class controls both drag-in and drag-out gestures. All the members of the NativeDragManager class are static, do not create an instance of this class.

The Clipboard object

Data that is dragged into or out of an application or component is contained in a Clipboard object. A single Clipboard object can make available different representations of the same information to increase the likelihood that another application can understand and use the data. For example, an image could be included as image data, a serialized Bitmap object, and as a file. Rendering of the data in a format can be deferred to a rendering function that is not called until the data is read.

Once a drag gesture has started, the Clipboard object can only be accessed from within an event handler for the nativeDragEnter, nativeDragOver, and nativeDragDrop events. After the drag gesture has ended, the Clipboard object cannot be read or reused.

An application object can be transferred as a reference and as a serialized object. References are only valid within the originating application. Serialized object transfers are valid between AIR applications, but can only be used with objects that remain valid when serialized and deserialized. Objects that are serialized are converted into the Action Message Format for ActionScript 3 (AMF3), a string-based data-transfer format.

Working with the Flex framework

In most cases, it is better to use the Adobe® Flex™ drag-and-drop API when building Flex applications. The Flex framework provides an equivalent feature set when a Flex application is run in AIR (it uses the AIR NativeDragManager internally). Flex also maintains a more limited feature set when an application or component is running within the more restrictive browser environment. AIR classes cannot be used in components or applications that run outside the AIR run-time environment.