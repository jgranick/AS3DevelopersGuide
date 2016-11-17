## Manipulating an HTML stylesheet from ActionScript {#manipulating-an-html-stylesheet-from-actionscript}

Adobe AIR 1.0 and later

Once the HTMLLoader object has dispatched the complete event, you can examine and manipulate CSS styles in a page.

For example, consider the following simple HTML document:

&lt;html&gt;

&lt;style&gt;

.style1A { font-family:Arial; font-size:12px }

.style1B { font-family:Arial; font-size:24px }

&lt;/style&gt;

&lt;style&gt;

.style2 { font-family:Arial; font-size:12px }

&lt;/style&gt;

&lt;body&gt;

&lt;p class=&quot;style1A&quot;&gt; Style 1A

&lt;/p&gt;

&lt;p class=&quot;style1B&quot;&gt; Style 1B

&lt;/p&gt;

&lt;p class=&quot;style2&quot;&gt; Style 2

&lt;/p&gt;

&lt;/body&gt;

&lt;/html&gt;

After an HTMLLoader object loads this content, you can manipulate the CSS styles in the page via the cssRules array of the window.document.styleSheets array, as shown here:

var html:HTMLLoader = new HTMLLoader( );

var urlReq:URLRequest = new URLRequest(&quot;test.html&quot;); html.load(urlReq); html.addEventListener(Event.COMPLETE, completeHandler); function completeHandler(event:Event):void {

var styleSheet0:Object = html.window.document.styleSheets[0]; styleSheet0.cssRules[0].style.fontSize = &quot;32px&quot;; styleSheet0.cssRules[1].style.color = &quot;#FF0000&quot;;

var styleSheet1:Object = html.window.document.styleSheets[1]; styleSheet1.cssRules[0].style.color = &quot;blue&quot;; styleSheet1.cssRules[0].style.font-family = &quot;Monaco&quot;;

}

This code adjusts the CSS styles so that the resulting HTML document appears like the following:

Keep in mind that code can add styles to the page after the HTMLLoader object dispatches the complete event.