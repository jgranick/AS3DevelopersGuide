## Content {#content}

You can load content into a StageWebView object using two methods: loadURL() and loadString().

The loadURL() method loads a resource at the specified URL. You can use any URI scheme supported by the system web browser control, including: data:, file:, http:, https:, and javascript:. The app: and app-storage: schemes are not supported. AIR does not validate the URL string.

The loadString() method loads a literal string containing HTML content. The location of a page loaded with this method is expressed as:

• On Desktop: about:blank

• On iOS: _htmlString_

• On Android: the data URI format of the encoded _htmlString_

The URI scheme determines the rules for loading embedded content or data.

| **URI scheme** | **Load local resource** | **Load remote resource** | **Local XMLHttpRequest** | **Remote XMLHttpRequest** |
| --- | --- | --- | --- | --- |
| data: | No | Yes | No | No |
| file: | Yes | Yes | Yes | Yes |
| http:, https: | No | Yes | No | Same domain |
| about: (loadString() method) | No | Yes | No | No |

**_Note:_ **_If the stage’s displayState property is set to FULL_SCREEN, in Desktop, you cannot type in a text field displayed in the StageWebView. However, in iOS and Android, you can type in a text field on StageWebView even if the stage’s displayState is FULL_SCREEN._

The following example uses a StageWebView object to display Adobe’s website:

package {

import flash.display.MovieClip; import flash.media.StageWebView; import flash.geom.Rectangle;

public class StageWebViewExample extends MovieClip{ var webView:StageWebView = new StageWebView(); public function StageWebViewExample() {

webView.stage = this.stage;

webView.viewPort = new Rectangle( 0, 0, stage.stageWidth, stage.stageHeight ); webView.loadURL( [&quot;http://www.adobe.com&quot;](http://www.adobe.com/) );

}

}

}

On Android devices, you must specify the Android INTERNET permission in order for the app to successfully load remote resources.

In Android 3.0+, an application must enable hardware acceleration in the Android manifestAdditions element of the AIR application descriptor to display plug-in content in a StageWebView object. See Enabling Flash Player and other plug-ins in a StageWebView object.

### JavaScript URI {#javascript-uri}

You can use a JavaScript URI to call a function defined in the HTML page that is loaded by a StageWebView object. The function you call using the JavaScript URI runs in the context of the loaded web page. The following example uses a StageWebView object to call a JavaScript function:

package {

import flash.display.*; import flash.geom.Rectangle;

import flash.media.StageWebView; public class WebView extends Sprite

{

public var webView:StageWebView = new StageWebView(); public function WebView()

{

var htmlString:String = &quot;&lt;!DOCTYPE HTML&gt;&quot; + &quot;&lt;html&gt;&lt;script type=text/javascript&gt;&quot; + &quot;function callURI(){&quot; +

&quot;alert(\&quot;You clicked me!!\&quot;);&quot;+ &quot;}&lt;/script&gt;&lt;body&gt;&quot; +

&quot;&lt;p&gt;&lt;a href=javascript:callURI()&gt;Click Me&lt;/a&gt;&lt;/p&gt;&quot; + &quot;&lt;/body&gt;&lt;/html&gt;&quot;;

webView.stage = this.stage;

webView.viewPort = new Rectangle( 0, 0, stage.stageWidth, stage.stageHeight ); webView.loadString( htmlString );

}

}

}

More Help topics

[Sean Voisen: Making the Most of StageWebView](http://voisen.org/blog/2010/10/making-the-most-of-stagewebview/)