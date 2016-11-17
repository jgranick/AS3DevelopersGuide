## Scrolling HTML content {#scrolling-html-content}

Adobe AIR 1.0 and later

The HTMLLoader class includes the following properties that let you control the scrolling of HTML content:

| **Property** | **Description** |
| --- | --- |
| contentHeight | The height, in pixels, of the HTML content. |
| contentWidth | The width, in pixels, of the HTML content. |
| scrollH | The horizontal scroll position of the HTML content within the HTMLLoader object. |
| scrollV | The vertical scroll position of the HTML content within the HTMLLoader object. |

The following code sets the scrollV property so that HTML content is scrolled to the bottom of the page:

var html:HTMLLoader = new HTMLLoader(); html.addEventListener(Event.HTML_BOUNDS_CHANGE, scrollHTML);

const SIZE:Number = 600; html.width = SIZE; html.height = SIZE;

var urlReq:URLRequest = new [URLRequest(&quot;http://www.adobe.com&quot;);](http://www.adobe.com/) html.load(urlReq);

this.addChild(html);

function scrollHTML(event:Event):void

{

html.scrollV = html.contentHeight - SIZE;

}

The HTMLLoader does not include horizontal and vertical scroll bars. You can implement scroll bars in ActionScript or by using a Flex component. The Flex HTML component automatically includes scroll bars for HTML content. You can also use the HTMLLoader.createRootWindow() method to create a window that contains an HTMLLoader object with scroll bars (see

“Creating windows with scrolling HTML content” on page 1018

).