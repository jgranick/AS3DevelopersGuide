## Defining browser-like user interfaces for HTML content {#defining-browser-like-user-interfaces-for-html-content}

Adobe AIR 1.0 and later

JavaScript provides several APIs for controlling the window displaying the HTML content. In AIR, these APIs can be overridden by implementing a custom [HTMLHost](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/html/HTMLHost.html) class.

**About extending the HTMLHost class**

Adobe AIR 1.0 and later

If, for example, your application presents multiple HTMLLoader objects in a tabbed interface, you may want title changes made by the loaded HTML pages to change the label of the tab, not the title of the main window. Similarly, your code could respond to a window.moveTo() call by repositioning the HTMLLoader object in its parent display object container, by moving the window that contains the HTMLLoader object, by doing nothing at all, or by doing something else entirely.

The AIR HTMLHost class controls the following JavaScript properties and methods:

• window.status

• window.document.title

• window.location

• window.blur()

• window.close()

• window.focus()

• window.moveBy()

• window.moveTo()

• window.open()

• window.resizeBy()

• window.resizeTo()

When you create an HTMLLoader object using new HTMLLoader(), the listed JavaScript properties or methods are not enabled. The HTMLHost class provides a default, browser-like implementation of these JavaScript APIs. You can also extend the HTMLHost class to customize the behavior. To create an HTMLHost object supporting the default behavior, set the defaultBehaviors parameter to true in the HTMLHost constructor:

var defaultHost:HTMLHost = new HTMLHost(true);

When you create an HTML window in AIR with the HTMLLoader class createRootWindow() method, an HTMLHost instance supporting the default behaviors is assigned automatically. You can change the host object behavior by assigning a different HTMLHost implementation to the htmlHost property of the HTMLLoader, or you can assign null to disable the features entirely.

**_Note:_ **_AIR assigns a default HTMLHost object to the initial window created for an HTML-based AIR application and any windows created by the default implementation of the JavaScript window.open() method._

### Example: Extending the HTMLHost class {#example-extending-the-htmlhost-class}

Adobe AIR 1.0 and later

The following example shows how to customize the way that an HTMLLoader object affects the user interface, by extending the HTMLHost class:

Flex example:

1.  Create a class that extends the HTMLHost class (a subclass).
2.  Override methods of the new class to handle changes in the user interface-related settings. For example, the following class, CustomHost, defines behaviors for calls to window.open() and changes to window.document.title. Calls to window.open() open the HTML page in a new window, and changes to window.document.title (including the setting of the &lt;title&gt; element of an HTML page) set the title of that window.

package

{

import flash.html.*;

import flash.display.StageScaleMode; import flash.display.NativeWindow;

import flash.display.NativeWindowInitOptions;

public class CustomHost extends HTMLHost

{

import flash.html.*; override public function

createWindow(windowCreateOptions:HTMLWindowCreateOptions):HTMLLoader

{

var initOptions:NativeWindowInitOptions = new NativeWindowInitOptions(); var bounds:Rectangle = new Rectangle(windowCreateOptions.x,

windowCreateOptions.y, windowCreateOptions.width, windowCreateOptions.height);

var htmlControl:HTMLLoader = HTMLLoader.createRootWindow(true, initOptions,

windowCreateOptions.scrollBarsVisible, bounds); htmlControl.htmlHost = new HTMLHostImplementation(); if(windowCreateOptions.fullscreen){

htmlControl.stage.displayState = StageDisplayState.FULL_SCREEN_INTERACTIVE;

}

return htmlControl;

}

override public function updateTitle(title:String):void

{

htmlLoader.stage.nativeWindow.title = title;

}

}

}

1.  In the code that contains the HTMLLoader (not the code of the new subclass of HTMLHost), create an object of the new class. Assign the new object to the htmlHost property of the HTMLLoader. The following Flex code uses the CustomHost class defined in the previous step:

&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;

&lt;mx:WindowedApplication [xmlns:mx=&quot;http://www.adobe.com/2006/mxml&quot;](http://www.adobe.com/2006/mxml) layout=&quot;vertical&quot; applicationComplete=&quot;init()&quot;&gt;

&lt;mx:Script&gt;

&lt;![CDATA[

import flash.html.HTMLLoader; import CustomHost;

private function init():void

{

}

]]&gt;

var html:HTMLLoader = new HTMLLoader(); html.width = container.width; html.height = container.height;

var urlReq:URLRequest = new URLRequest(&quot;Test.html&quot;); html.htmlHost = new CustomHost();

html.load(urlReq); container.addChild(html);

&lt;/mx:Script&gt;

&lt;mx:UIComponent id=&quot;container&quot; width=&quot;100%&quot; height=&quot;100%&quot;/&gt;

&lt;/mx:WindowedApplication&gt;

To test the code described here, include an HTML file with the following content in the application directory:

&lt;html&gt;

&lt;head&gt;

&lt;title&gt;Test&lt;/title&gt;

&lt;/head&gt;

&lt;script&gt;

function openWindow()

{

window.runtime.trace(&quot;in&quot;); document.title = &quot;foo&quot; window.open(&#039;Test.html&#039;); window.runtime.trace(&quot;out&quot;);

}

&lt;/script&gt;

&lt;body&gt;

&lt;a href=&quot;#&quot; onclick=&quot;openWindow()&quot;&gt;window.open(&#039;Test.html&#039;)&lt;/a&gt;

&lt;/body&gt;

&lt;/html&gt;

Flash Professional example:

1.  Create a Flash file for AIR. Set its document class to CustomHostExample and then save the file as CustomHostExample.fla.
2.  Create an ActionScript file called CustomHost.as containing a class that extends the HTMLHost class (a subclass). This class overrides certain methods of the new class to handle changes in the user interface-related settings. For example, the following class, CustomHost, defines behaviors for calls to window.open() and changes to window.document.title. Calls to the window.open() method open the HTML page in a new window, and changes to the window.document.title property (including the setting of the &lt;title&gt; element of an HTML page) set the title of that window.

package

{

import flash.display.StageScaleMode; import flash.display.NativeWindow;

import flash.display.NativeWindowInitOptions; import flash.events.Event;

import flash.events.NativeWindowBoundsEvent; import flash.geom.Rectangle;

import flash.html.HTMLLoader; import flash.html.HTMLHost;

import flash.html.HTMLWindowCreateOptions; import flash.text.TextField;

public class CustomHost extends HTMLHost

{

public var statusField:TextField;

public function CustomHost(defaultBehaviors:Boolean=true)

{

super(defaultBehaviors);

}

override public function windowClose():void

{

htmlLoader.stage.nativeWindow.close();

}

override public function createWindow(

windowCreateOptions:HTMLWindowCreateOptions ):HTMLLoader

{

var initOptions:NativeWindowInitOptions = new NativeWindowInitOptions(); var bounds:Rectangle = new Rectangle(windowCreateOptions.x,

windowCreateOptions.y, windowCreateOptions.width, windowCreateOptions.height);

var htmlControl:HTMLLoader = HTMLLoader.createRootWindow(true, initOptions,

windowCreateOptions.scrollBarsVisible, bounds); htmlControl.htmlHost = new HTMLHostImplementation(); if(windowCreateOptions.fullscreen){

htmlControl.stage.displayState = StageDisplayState.FULL_SCREEN_INTERACTIVE;

}

return htmlControl;

}

override public function updateLocation(locationURL:String):void

{

trace(locationURL);

}

override public function set windowRect(value:Rectangle):void

{

htmlLoader.stage.nativeWindow.bounds = value;

}

override public function updateStatus(status:String):void

{

statusField.text = status; trace(status);

}

override public function updateTitle(title:String):void

{

htmlLoader.stage.nativeWindow.title = title + &quot;- Example Application&quot;;

}

override public function windowBlur():void

{

htmlLoader.alpha = 0.5;

}

override public function windowFocus():void

{

htmlLoader.alpha = 1;

}

}

}

1.  Create another ActionScript file named CustomHostExample.as to contain the document class for the application. This class creates an HTMLLoader object and sets its host property to an instance of the CustomHost class defined in the previous step:

package

{

import flash.display.Sprite; import flash.html.HTMLLoader; import flash.net.URLRequest; import flash.text.TextField;

public class CustomHostExample extends Sprite

{

function CustomHostExample():void

{

var html:HTMLLoader = new HTMLLoader(); html.width = 550;

html.height = 380;

var host:CustomHost = new CustomHost(); html.htmlHost = host;

var urlReq:URLRequest = new URLRequest(&quot;Test.html&quot;); html.load(urlReq);

addChild(html);

var statusTxt:TextField = new TextField(); statusTxt.y = 380;

statusTxt.height = 20;

statusTxt.width = 550; statusTxt.background = true; statusTxt.backgroundColor = 0xEEEEEEEE; addChild(statusTxt);

host.statusField = statusTxt;

}

}

}

To test the code described here, include an HTML file with the following content in the application directory:

&lt;html&gt;

&lt;head&gt;

&lt;title&gt;Test&lt;/title&gt;

&lt;script&gt;

function openWindow()

{

document.title = &quot;Test&quot; window.open(&#039;Test.html&#039;);

}

&lt;/script&gt;

&lt;/head&gt;

&lt;body bgColor=&quot;#EEEEEE&quot;&gt;

&lt;a href=&quot;#&quot; onclick=&quot;window.open(&#039;Test.html&#039;)&quot;&gt;window.open(&#039;Test.html&#039;)&lt;/a&gt;

&lt;br/&gt;&lt;a href=&quot;#&quot; onclick=&quot;window.document.location=&#039;http://www.adobe.com&#039;&quot;&gt; window.document.location = &#039;http://www.adobe.com&#039;&lt;/a&gt;

&lt;br/&gt;&lt;a href=&quot;#&quot; onclick=&quot;window.moveBy(6, 12)&quot;&gt;moveBy(6, 12)&lt;/a&gt;

&lt;br/&gt;&lt;a href=&quot;#&quot; onclick=&quot;window.close()&quot;&gt;window.close()&lt;/a&gt;

&lt;br/&gt;&lt;a href=&quot;#&quot; onclick=&quot;window.blur()&quot;&gt;window.blur()&lt;/a&gt;

&lt;br/&gt;&lt;a href=&quot;#&quot; onclick=&quot;window.focus()&quot;&gt;window.focus()&lt;/a&gt;

&lt;br/&gt;&lt;a href=&quot;#&quot; onclick=&quot;window.status = new Date().toString()&quot;&gt;window.status=new Date().toString()&lt;/a&gt;

&lt;/body&gt;

&lt;/html&gt;

### Handling changes to the window.location property {#handling-changes-to-the-window-location-property}

Adobe AIR 1.0 and later

Override the locationChange() method to handle changes of the URL of the HTML page. The locationChange() method is called when JavaScript in a page changes the value of window.location. The following example simply loads the requested URL:

override public function updateLocation(locationURL:String):void

{

htmlLoader.load(new URLRequest(locationURL));

}

**_Note:_ **_You can use the htmlLoader property of the HTMLHost object to reference the current HTMLLoader object._

### Handling JavaScript calls to window.moveBy(), window.moveTo(), window.resizeTo(), window.resizeBy() {#handling-javascript-calls-to-window-moveby-window-moveto-window-resizeto-window-resizeby}

Adobe AIR 1.0 and later

Override the set windowRect() method to handle changes in the bounds of the HTML content. The set windowRect() method is called when JavaScript in a page calls window.moveBy(), window.moveTo(), window.resizeTo(), or window.resizeBy(). The following example simply updates the bounds of the desktop window:

override public function set windowRect(value:Rectangle):void

{

htmlLoader.stage.nativeWindow.bounds = value;

}

### Handling JavaScript calls to window.open() {#handling-javascript-calls-to-window-open}

Adobe AIR 1.0 and later

Override the createWindow() method to handle JavaScript calls to window.open(). Implementations of the createWindow() method are responsible for creating and returning a new HTMLLoader object. Typically, you would display the HTMLLoader in a new window, but creating a window is not required.

The following example illustrates how to implement the createWindow() function using the HTMLLoader.createRootWindow() to create both the window and the HTMLLoader object. You can also create a NativeWindow object separately and add the HTMLLoader to the window stage.

override public function createWindow(windowCreateOptions:HTMLWindowCreateOptions):HTMLLoader{ var initOptions:NativeWindowInitOptions = new NativeWindowInitOptions();

var bounds:Rectangle = new Rectangle(windowCreateOptions.x, windowCreateOptions.y,

windowCreateOptions.width, windowCreateOptions.height); var htmlControl:HTMLLoader = HTMLLoader.createRootWindow(true, initOptions,

windowCreateOptions.scrollBarsVisible, bounds); htmlControl.htmlHost = new HTMLHostImplementation(); if(windowCreateOptions.fullscreen){

htmlControl.stage.displayState = StageDisplayState.FULL_SCREEN_INTERACTIVE;

}

return htmlControl;

}

**_Note:_ **_This example assigns the custom HTMLHost implementation to any new windows created with window.open(). You can also use a different implementation or set the htmlHost property to null for new windows, if desired._

The object passed as a parameter to the createWindow() method is an [HTMLWindowCreateOptions](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/html/HTMLWindowCreateOptions.html) object. The HTMLWindowCreateOptions class includes properties that report the values set in the features parameter string in the call to window.open():

| **HTMLWindowCreateOptions property** | **Corresponding setting in the features string in the JavaScript call to window.open()** |
| --- | --- |
| fullscreen | fullscreen |
| height | height |
| locationBarVisible | location |
| menuBarVisible | menubar |
| resizeable | resizable |
| scrollBarsVisible | scrollbars |
| statusBarVisible | status |
| toolBarVisible | toolbar |
| width | width |
| x | left or screenX |
| y | top or screenY |

The HTMLLoader class does not implement all the features that can be specified in the feature string. Your application must provide scroll bars, location bars, menu bars, status bars, and toolbars when appropriate.

The other arguments to the JavaScript window.open() method are handled by the system. A createWindow()

implementation should not load content in the HTMLLoader object, or set the window title.

### Handling JavaScript calls to window.close() {#handling-javascript-calls-to-window-close}

Adobe AIR 1.0 and later

Override the windowClose() to handle JavaScript calls to window.close() method. The following example closes the desktop window when the window.close() method is called:

override public function windowClose():void

{

htmlLoader.stage.nativeWindow.close();

}

JavaScript calls to window.close() do not have to close the containing window. You could, for example, remove the HTMLLoader from the display list, leaving the window (which may have other content) open, as in the following code:

override public function windowClose():void

{

htmlLoader.parent.removeChild(htmlLoader);

}

### Handling changes of the window.status property {#handling-changes-of-the-window-status-property}

Adobe AIR 1.0 and later

Override the updateStatus() method to handle JavaScript changes to the value of window.status. The following example traces the status value:

override public function updateStatus(status:String):void

{

trace(status);

}

The requested status is passed as a string to the updateStatus() method. The HTMLLoader object does not provide a status bar.

### Handling changes of the window.document.title property {#handling-changes-of-the-window-document-title-property}

Adobe AIR 1.0 and later

override the updateTitle() method to handle JavaScript changes to the value of window.document.title. The following example changes the window title and appends the string, &quot;Sample,&quot; to the title:

override public function updateTitle(title:String):void

{

htmlLoader.stage.nativeWindow.title = title + &quot; - Sample&quot;;

}

When document.title is set on an HTML page, the requested title is passed as a string to the updateTitle()

method.

Changes to document.title do not have to change the title of the window containing the HTMLLoader object. You could, for example, change another interface element, such as a text field.

### Handling JavaScript calls to window.blur() and window.focus() {#handling-javascript-calls-to-window-blur-and-window-focus}

Adobe AIR 1.0 and later

Override the windowBlur() and windowFocus() methods to handle JavaScript calls to window.blur() and

window.focus(), as shown in the following example:

override public function windowBlur():void

{

htmlLoader.alpha = 0.5;

}

override public function windowFocus():void

{

htmlLoader.alpha = 1.0; NativeApplication.nativeApplication.activate(htmlLoader.stage.nativeWindow);

}

**_Note:_ **_AIR does not provide an API for deactivating a window or application._

### Creating windows with scrolling HTML content {#creating-windows-with-scrolling-html-content}

Adobe AIR 1.0 and later

The HTMLLoader class includes a static method, HTMLLoader.createRootWindow(), which lets you open a new window (represented by a NativeWindow object) that contains an HTMLLoader object and define some user interface settings for that window. The method takes four parameters, which let you define the user interface:

| **Parameter** | **Description** |
| --- | --- |
| visible | A Boolean value that specifies whether the window is initially visible (true) or not (false). |
| windowInitOptions | A NativeWindowInitOptions object. The NativeWindowInitOptions class defines initialization options for a NativeWindow object, including the following: whether the window is minimizable, maximizable, or resizable, whether the window has system chrome or custom chrome, whether the window is transparent or not (for windows that do not use system chrome), and the type of window. |
| scrollBarsVisible | Whether there are scroll bars (true) or not (false). |
| bounds | A Rectangle object defining the position and size of the new window. |

For example, the following code uses the HTMLLoader.createRootWindow() method to create a window with HTMLLoader content that uses scroll bars:

var initOptions:NativeWindowInitOptions = new NativeWindowInitOptions(); var bounds:Rectangle = new Rectangle(10, 10, 600, 400);

var html2:HTMLLoader = HTMLLoader.createRootWindow(true, initOptions, true, bounds); var urlReq2:URLRequest = new [URLRequest(&quot;http://www.example.com&quot;);](http://www.example.com/) html2.load(urlReq2);

html2.stage.nativeWindow.activate();

**_Note:_ **_Windows created by calling createRootWindow() directly in JavaScript remain independent from the opening HTML window. The JavaScript Window opener and parent properties, for example, are null. However, if you call createRootWindow() indirectly by overriding the HTMLHost createWindow() method to call createRootWindow(), then opener and parent do reference the opening HTML window._