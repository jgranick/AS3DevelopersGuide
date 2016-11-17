## Avoiding security-related JavaScript errors {#avoiding-security-related-javascript-errors}

Adobe AIR 1.0 and later

If you call code that is restricted from use in a sandbox due to these security restrictions, the runtime dispatches a JavaScript error: “Adobe AIR runtime security violation for JavaScript code in the application security sandbox.” To avoid this error, follow these coding practices.

**Causes of security-related JavaScript errors**

Adobe AIR 1.0 and later

Code executing in the application sandbox is restricted from most operations that involve evaluating and executing strings once the document load event has fired and any load event handlers have exited. Attempting to use the following types of JavaScript statements that evaluate and execute potentially insecure strings generates JavaScript errors:

•

eval() function

•

setTimeout() and setInterval()

•

Function constructor

In addition, the following types of JavaScript statements fail without generating an unsafe JavaScript error:

•

javascript: URLs

•

Event callbacks assigned through onevent attributes in innerHTML and outerHTML statements

•

Loading JavaScript files from outside the application installation directory

•

document.write() and document.writeln()

•

Synchronous XMLHttpRequests before the load event or during a load event handler

•

Dynamically created script elements

**_Note:_ **_In some restricted cases, evaluation of strings is permitted. See_

_“Code restrictions for content in different_

_sandboxes” on page 1082_

_for more information._

Adobe maintains a list of Ajax frameworks known to support the application security sandbox, at [http://www.adobe.com/go/airappsandboxframeworks](http://www.adobe.com/go/airappsandboxframeworks).

The following sections describe how to rewrite scripts to avoid these unsafe JavaScript errors and silent failures for code running in the application sandbox.

### Mapping application content to a different sandbox {#mapping-application-content-to-a-different-sandbox}

Adobe AIR 1.0 and later

In most cases, you can rewrite or restructure an application to avoid security-related JavaScript errors. However, when rewriting or restructuring is not possible, you can load the application content into a different sandbox using the technique described in

“Loading application content into a non-application sandbox” on page 999

. If that content also must access AIR APIs, you can create a sandbox bridge, as described in

“Setting up a sandbox bridge interface” on

page 1000

.

### eval() function {#eval-function}

Flash Player 9 and later, Adobe AIR 1.0 and later

In the application sandbox, the eval() function can only be used before the page load event or during a load event handler. After the page has loaded, calls to eval() will not execute code. However, in the following cases, you can rewrite your code to avoid the use of eval().

### Assigning properties to an object {#assigning-properties-to-an-object}

Adobe AIR 1.0 and later

Instead of parsing a string to build the property accessor:

eval(&quot;obj.&quot; + propName + &quot; = &quot; + val); access properties with bracket notation: obj[propName] = val;

### Creating a function with variables available in context {#creating-a-function-with-variables-available-in-context}

Adobe AIR 1.0 and later

Replace statements such as the following:

function compile(var1, var2){

eval(&quot;var fn = function(){ this.&quot;+var1+&quot;(var2) }&quot;); return fn;

}

with:

function compile(var1, var2){ var self = this;

return function(){ self[var1](var2) };

}

### Creating an object using the name of the class as a string parameter {#creating-an-object-using-the-name-of-the-class-as-a-string-parameter}

Adobe AIR 1.0 and later

Consider a hypothetical JavaScript class defined with the following code:

var CustomClass =

{

Utils:

{

Parser: function(){ alert(&#039;constructor&#039;) }

},

Data:

{

}

};

var constructorClassName = &quot;CustomClass.Utils.Parser&quot;;

The simplest way to create a instance would be to use eval(): var myObj;

eval(&#039;myObj=new &#039; + constructorClassName +&#039;()&#039;)

However, you could avoid the call to eval() by parsing each component of the class name and building the new object using bracket notation:

function getter(str)

{

var obj = window;

var names = str.split(&#039;.&#039;); for(var i=0;i&lt;names.length;i++){

if(typeof obj[names[i]]==&#039;undefined&#039;){ var undefstring = names[0]; for(var j=1;j&lt;=i;j++)

undefstring+=&quot;.&quot;+names[j];

throw new Error(undefstring+&quot; is undefined&quot;);

}

obj = obj[names[i]];

}

return obj;

}

To create the instance, use:

try{

var Parser = getter(constructorClassName); var a = new Parser();

}catch(e){

alert(e);

}

### setTimeout() and setInterval() {#settimeout-and-setinterval}

Adobe AIR 1.0 and later

Replace the string passed as the handler function with a function reference or object. For example, replace a statement such as:

setTimeout(&quot;alert(&#039;Timeout&#039;)&quot;, 100);

with:

setTimeout(function(){alert(&#039;Timeout&#039;)}, 100);

Or, when the function requires the this object to be set by the caller, replace a statement such as:

this.appTimer = setInterval(&quot;obj.customFunction();&quot;, 100);

with the following:

var _self = this;

this.appTimer = setInterval(function(){obj.customFunction.apply(_self);}, 100);

### Function constructor {#function-constructor}

Adobe AIR 1.0 and later

Calls to new Function(param, body) can be replaced with an inline function declaration or used only before the page load event has been handled.

### javascript: URLs {#javascript-urls}

Adobe AIR 1.0 and later

The code defined in a link using the javascript: URL scheme is ignored in the application sandbox. No unsafe JavaScript error is generated. You can replace links using javascript: URLs, such as:

&lt;a href=&quot;javascript:code()&quot;&gt;Click Me&lt;/a&gt;

with:

&lt;a href=&quot;#&quot; onclick=&quot;code()&quot;&gt;Click Me&lt;/a&gt;

### Event callbacks assigned through onevent attributes in innerHTML and outerHTML statements {#event-callbacks-assigned-through-onevent-attributes-in-innerhtml-and-outerhtml-statements}

Adobe AIR 1.0 and later

When you use innerHTML or outerHTML to add elements to the DOM of a document, any event callbacks assigned within the statement, such as onclick or onmouseover, are ignored. No security error is generated. Instead, you can assign an id attribute to the new elements and set the event handler callback functions using the addEventListener() method.

For example, given a target element in a document, such as:

&lt;div id=&quot;container&quot;&gt;&lt;/div&gt;

Replace statements such as:

document.getElementById(&#039;container&#039;).innerHTML = &#039;&lt;a href=&quot;#&quot; onclick=&quot;code()&quot;&gt;Click Me.&lt;/a&gt;&#039;;

with:

document.getElementById(&#039;container&#039;).innerHTML = &#039;&lt;a href=&quot;#&quot; id=&quot;smith&quot;&gt;Click Me.&lt;/a&gt;&#039;; document.getElementById(&#039;smith&#039;).addEventListener(&quot;click&quot;, function() { code(); });

### Loading JavaScript files from outside the application installation directory {#loading-javascript-files-from-outside-the-application-installation-directory}

Adobe AIR 1.0 and later

Loading script files from outside the application sandbox is not permitted. No security error is generated. All script files that run in the application sandbox must be installed in the application directory. To use external scripts in a page, you must map the page to a different sandbox. See

“Loading application content into a non-application sandbox” on

page 999

.

### document.write() and document.writeln() {#document-write-and-document-writeln}

Adobe AIR 1.0 and later

Calls to document.write() or document.writeln() are ignored after the page load event has been handled. No security error is generated. As an alternative, you can load a new file, or replace the body of the document using DOM manipulation techniques.

### Synchronous XMLHttpRequests before the load event or during a load event handler {#synchronous-xmlhttprequests-before-the-load-event-or-during-a-load-event-handler}

Adobe AIR 1.0 and later

Synchronous XMLHttpRequests initiated before the page load event or during a load event handler do not return any content. Asynchronous XMLHttpRequests can be initiated, but do not return until after the load event. After the load event has been handled, synchronous XMLHttpRequests behave normally.

### Dynamically created script elements {#dynamically-created-script-elements}

Adobe AIR 1.0 and later

Dynamically created script elements, such as when created with innerHTML or document.createElement()

method are ignored.