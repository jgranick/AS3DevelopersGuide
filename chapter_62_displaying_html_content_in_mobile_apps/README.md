# Chapter 62: Displaying HTML content in mobile apps {#chapter-62-displaying-html-content-in-mobile-apps}

Adobe AIR 2.5 and later

The StageWebView class displays HTML content using the system browser control on mobile devices and using the standard Adobe® AIR® HTMLLoader control on desktop computers. Check the StageWebView.isSupported property to determine whether the class is supported on the current device. Support is not guaranteed for all devices in the mobile profile.

In all profiles, the StageWebView class supports only limited interaction between the HTML content and the rest of the application. You can control navigation, but no cross-scripting or direct exchange of data is allowed. You can load content from a local or remote URL or pass in a string of HTML.

**StageWebView objects**

A StageWebView object is not a display object and cannot be added to the display list. Instead, it operates as a viewport attached directly to the stage. StageWebView content draws over the top of any display list content. There is no way to control the drawing order of multiple StageWebView objects.

To display a StageWebView object, you assign the stage on which the object is to appear to the stage property of the StageWebView. Set the size of the display using the viewPort property.

Set the x and y coordinates of the viewPort property between -8192 and 8191\. The maximum value of stage width and height is 8191\. If the size exceeds the maximum values, an exception is thrown.

The following example creates a StageWebView object, sets the stage and viewPort properties, and displays a string of HTML:

var webView:StageWebView = new StageWebView();

webView.viewPort = new Rectangle( 0, 0, this.stage.stageWidth, this .stage.stageHeight); webView.stage = this.stage;

var htmlString:String = &quot;&lt;!DOCTYPE HTML&gt;&quot; +

&quot;&lt;html&gt;&lt;body&gt;&quot; +

&quot;&lt;p&gt;King Philip could order five good steaks.&lt;/p&gt;&quot; + &quot;&lt;/body&gt;&lt;/html&gt;&quot;;

webView.loadString( htmlString );

To hide a StageWebView object, set its stage property to null. To destroy the object entirely, call the dispose() method. Calling dispose() is optional, but does help the garbage collector reclaim the memory used by the object sooner.