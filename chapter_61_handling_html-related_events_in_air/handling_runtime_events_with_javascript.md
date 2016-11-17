## Handling runtime events with JavaScript {#handling-runtime-events-with-javascript}

Adobe AIR 1.0 and later

The runtime classes support adding event handlers with the addEventListener() method. To add a handler function for an event, call the addEventListener() method of the object that dispatches the event, providing the event type and the handling function. For example, to listen for the closing event dispatched when a user clicks the window close button on the title bar, use the following statement:

window.nativeWindow.addEventListener(air.NativeWindow.CLOSING, handleWindowClosing);

**Creating an event handler function**

Adobe AIR 1.0 and later

The following code creates a simple HTML file that displays information about the position of the main window. A handler function named moveHandler(), listens for a move event (defined by the NativeWindowBoundsEvent class) of the main window.

&lt;html&gt;

&lt;script src=&quot;AIRAliases.js&quot; /&gt;

&lt;script&gt;

function init() { writeValues();

window.nativeWindow.addEventListener(air.NativeWindowBoundsEvent.MOVE,

moveHandler);

}

function writeValues() {

document.getElementById(&quot;xText&quot;).value = window.nativeWindow.x; document.getElementById(&quot;yText&quot;).value = window.nativeWindow.y;

}

function moveHandler(event) { air.trace(event.type); // move writeValues();

}

&lt;/script&gt;

&lt;body onload=&quot;init()&quot; /&gt;

&lt;table&gt;

&lt;tr&gt;

&lt;td&gt;Window X:&lt;/td&gt;

&lt;td&gt;&lt;textarea id=&quot;xText&quot;&gt;&lt;/textarea&gt;&lt;/td&gt;

&lt;/tr&gt;

&lt;tr&gt;

&lt;td&gt;Window Y:&lt;/td&gt;

&lt;td&gt;&lt;textarea id=&quot;yText&quot;&gt;&lt;/textarea&gt;&lt;/td&gt;

&lt;/tr&gt;

&lt;/table&gt;

&lt;/body&gt;

&lt;/html&gt;

When a user moves the window, the textarea elements display the updated X and Y positions of the window:

Notice that the event object is passed as an argument to the moveHandler() method. The event parameter allows your handler function to examine the event object. In this example, you use the event object&#039;s type property to report that the event is a move event.

**Removing event listeners**

Adobe AIR 1.0 and later

You can use the removeEventListener() method to remove an event listener that you no longer need. It is a good idea to remove any listeners that will no longer be used. Required parameters include the eventName and listener parameters, which are the same as the required parameters for the addEventListener() method.

Removing event listeners in HTML pages that navigate

Adobe AIR 1.0 and later

When HTML content navigates, or when HTML content is discarded because a window that contains it is closed, the event listeners that reference objects on the unloaded page are not automatically removed. When an object dispatches an event to a handler that has already been unloaded, you see the following error message: “The application attempted to reference a JavaScript object in an HTML page that is no longer loaded.”

To avoid this error, remove JavaScript event listeners in an HTML page before it goes away. In the case of page navigation (within an HTMLLoader object), remove the event listener during the unload event of the window object.

For example, the following JavaScript code removes an event listener for an uncaughtScriptException event:

window.onunload = cleanup; window.htmlLoader.addEventListener(&#039;uncaughtScriptException&#039;, uncaughtScriptException); function cleanup()

{

window.htmlLoader.removeEventListener(&#039;uncaughtScriptException&#039;,

uncaughtScriptExceptionHandler);

}

To prevent the error from occurring when closing windows that contain HTML content, call a cleanup function in response to the closing event of the NativeWindow object (window.nativeWindow). For example, the following JavaScript code removes an event listener for an uncaughtScriptException event:

window.nativeWindow.addEventListener(air.Event.CLOSING, cleanup); function cleanup()

{

window.htmlLoader.removeEventListener(&#039;uncaughtScriptException&#039;,

uncaughtScriptExceptionHandler);

}

You can also prevent this error from occurring by removing an event listener as soon as it runs (if the event only needs to be handled once). For example, the following JavaScript code creates an html window by calling the createRootWindow() method of the HTMLLoader class and adds an event listener for the complete event. When the complete event handler is called, it removes its own event listener using the removeEventListener() function:

var html = runtime.flash.html.HTMLLoader.createRootWindow(true); html.addEventListener(&#039;complete&#039;, htmlCompleteListener); function htmlCompleteListener()

{

html.removeEventListener(complete, arguments.callee)

// handler code..

}

html.load(new runtime.flash.net.URLRequest(&quot;second.html&quot;));

Removing unneeded event listeners also allows the system garbage collector to reclaim any memory associated with those listeners.

Checking for existing event listeners

Adobe AIR 1.0 and later

The hasEventListener() method lets you check for the existence of an event listener on an object.