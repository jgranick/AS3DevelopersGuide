## Using ActionScript libraries within an HTML page {#using-actionscript-libraries-within-an-html-page}

Adobe AIR 1.0 and later

AIR extends the HTML script element so that a page can import ActionScript classes in a compiled SWF file. For example, to import a library named, _myClasses.swf_, located in the lib subdirectory of the root application folder, include the following script tag within an HTML file:

&lt;script src=&quot;lib/myClasses.swf&quot; type=&quot;application/x-shockwave-flash&quot;&gt;&lt;/script&gt;

**_Important:_ **_The type attribute must be type=&quot;application/x-shockwave-flash&quot; for the library to be properly loaded._

If the SWF content is compiled as a Flash Player 10 or AIR 1.5 SWF, you must set the XML namespace of the application descriptor file to the AIR 1.5 namespace.

The lib directory and myClasses.swf file must also be included when the AIR file is packaged. Access the imported classes through the runtime property of the JavaScript Window object: var libraryObject = new window.runtime.LibraryClass();

If the classes in the SWF file are organized in packages, you must include the package name as well. For example, if the LibraryClass definition was in a package named _utilities_, you would create an instance of the class with the following statement:

var libraryObject = new window.runtime.utilities.LibraryClass();

**_Note:_ **_To compile an ActionScript SWF library for use as part of an HTML page in AIR, use the acompc compiler. The acompc utility is part of the Flex SDK and is described in the Flex SDK documentation._

**Accessing the HTML DOM and JavaScript objects from an imported ActionScript file**

Adobe AIR 1.0 and later

To access objects in an HTML page from ActionScript in a SWF file imported into the page using the &lt;script&gt; tag, pass a reference to a JavaScript object, such as window or document, to a function defined in the ActionScript code. Use the reference within the function to access the JavaScript object (or other objects accessible through the passed-in reference).

For example, consider the following HTML page:

&lt;html&gt;

&lt;script src=&quot;ASLibrary.swf&quot; type=&quot;application/x-shockwave-flash&quot;&gt;&lt;/script&gt;

&lt;script&gt;

num = 254;

function getStatus() { return &quot;OK.&quot;;

}

function runASFunction(window){

var obj = new runtime.ASClass(); obj.accessDOM(window);

}

&lt;/script&gt;

&lt;body onload=&quot;runASFunction&quot;&gt;

&lt;p id=&quot;p1&quot;&gt;Body text.&lt;/p&gt;

&lt;/body&gt;

&lt;/html&gt;

This simple HTML page has a JavaScript variable named _num_ and a JavaScript function named _getStatus()_. Both of these are properties of the window object of the page. Also, the window.document object includes a named P element (with the ID _p1_).

The page loads an ActionScript file, “ASLibrary.swf,” that contains a class, ASClass. ASClass defines a function named accessDOM() that simply traces the values of these JavaScript objects. The accessDOM() method takes the JavaScript Window object as an argument. Using this Window reference, it can access other objects in the page including variables, functions, and DOM elements as illustrated in the following definition:

public class ASClass{

public function accessDOM(window:*):void { trace(window.num); // 254

trace(window.document.getElementById(&quot;p1&quot;).innerHTML); // Body text.. trace(window.getStatus()); // OK.

}

}

You can both get and set properties of the HTML page from an imported ActionScript class. For example, the following function sets the contents of the p1 element on the page and it sets the value of the foo JavaScript variable on the page:

public function modifyDOM(window:*):void { window.document.getElementById(&quot;p1&quot;).innerHTML = &quot;Bye&quot;; window.foo = 66;