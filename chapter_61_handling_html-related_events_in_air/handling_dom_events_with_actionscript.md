## Handling DOM events with ActionScript {#handling-dom-events-with-actionscript}

Adobe AIR 1.0 and later

You can register ActionScript functions to respond to JavaScript events. For example, consider the following HTML content:

&lt;html&gt;

&lt;body&gt;

&lt;a href=&quot;#&quot; id=&quot;testLink&quot;&gt;Click me.&lt;/a&gt;

&lt;/html&gt;

You can register an ActionScript function as a handler for any event in the page. For example, the following code adds the clickHandler() function as the listener for the onclick event of the testLink element in the HTML page:

var html:HTMLLoader = new HTMLLoader( );

var urlReq:URLRequest = new URLRequest(&quot;test.html&quot;); html.load(urlReq); html.addEventListener(Event.COMPLETE, completeHandler);

function completeHandler(event:Event):void { html.window.document.getElementById(&quot;testLink&quot;).onclick = clickHandler;

}

function clickHandler( event:Object ):void { trace(&quot;Event of type: &quot; + event.type );

}

The event object dispatched is not of type flash.events.Event or one of the Event subclasses. Use the Object class to declare a type for the event handler function argument.

You can also use the addEventListener() method to register for these events. For example, you could replace the

completeHandler() method in the previous example with the following code:

function completeHandler(event:Event):void {

var testLink:Object = html.window.document.getElementById(&quot;testLink&quot;); testLink.addEventListener(&quot;click&quot;, clickHandler);

}

When a listener refers to a specific DOM element, it is good practice to wait for the parent HTMLLoader to dispatch the complete event before adding the event listeners. HTML pages often load multiple files and the HTML DOM is not fully built until all the files are loaded and parsed. The HTMLLoader dispatches the complete event when all elements have been created.