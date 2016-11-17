## Responding to uncaught JavaScript exceptions {#responding-to-uncaught-javascript-exceptions}

Adobe AIR 1.0 and later

Consider the following HTML:

&lt;html&gt;

&lt;head&gt;

&lt;script&gt;

function throwError() {

var x = 400 * melbaToast;

}

&lt;/script&gt;

&lt;/head&gt;

&lt;body&gt;

&lt;a href=&quot;#&quot; onclick=&quot;throwError()&quot;&gt;Click me.&lt;/a&gt;

&lt;/html&gt;

It contains a JavaScript function, throwError(), that references an unknown variable, melbaToast: var x = 400 * melbaToast;

When a JavaScript operation encounters an illegal operation that is not caught in the JavaScript code with a try/catch structure, the HTMLLoader object containing the page dispatches an HTMLUncaughtScriptExceptionEvent event. You can register a handler for this event, as in the following code:

var html:HTMLLoader = new HTMLLoader();

var urlReq:URLRequest = new URLRequest(&quot;test.html&quot;); html.load(urlReq);

html.width = container.width; html.height = container.height; container.addChild(html);

html.addEventListener(HTMLUncaughtScriptExceptionEvent.UNCAUGHT_SCRIPT_EXCEPTION,

htmlErrorHandler);

function htmlErrorHandler(event:HTMLUncaughtJavaScriptExceptionEvent):void

{

event.preventDefault(); trace(&quot;exceptionValue:&quot;, event.exceptionValue)

for (var i:int = 0; i &lt; event.stackTrace.length; i++)

{

trace(&quot;sourceURL:&quot;, event.stackTrace[i].sourceURL); trace(&quot;line:&quot;, event.stackTrace[i].line); trace(&quot;function:&quot;, event.stackTrace[i].functionName);

}

}

Within JavaScript, you can handle the same event using the window.htmlLoader property:

&lt;html&gt;

&lt;head&gt;

&lt;script language=&quot;javascript&quot; type=&quot;text/javascript&quot; src=&quot;AIRAliases.js&quot;&gt;&lt;/script&gt;

&lt;script&gt;

function throwError() {

var x = 400 * melbaToast;

}

function htmlErrorHandler(event) { event.preventDefault();

var message = &quot;exceptionValue:&quot; + event.exceptionValue + &quot;\n&quot;; for (var i = 0; i &lt; event.stackTrace.length; i++){

message += &quot;sourceURL:&quot; + event.stackTrace[i].sourceURL +&quot;\n&quot;; message += &quot;line:&quot; + event.stackTrace[i].line +&quot;\n&quot;;

message += &quot;function:&quot; + event.stackTrace[i].functionName + &quot;\n&quot;;

}

alert(message);

}

window.htmlLoader.addEventListener(&quot;uncaughtScriptException&quot;, htmlErrorHandler);

&lt;/script&gt;

&lt;/head&gt;

&lt;body&gt;

&lt;a href=&quot;#&quot; onclick=&quot;throwError()&quot;&gt;Click me.&lt;/a&gt;

&lt;/html&gt;

The htmlErrorHandler() event handler cancels the default behavior of the event (which is to send the JavaScript error message to the AIR trace output), and generates its own output message. It outputs the value of the exceptionValue of the HTMLUncaughtScriptExceptionEvent object. It outputs the properties of each object in the stackTrace array:

exceptionValue: ReferenceError: Can&#039;t find variable: melbaToast sourceURL: app:/test.html

line: 5

function: throwError sourceURL: app:/test.html line: 10

function: onclick