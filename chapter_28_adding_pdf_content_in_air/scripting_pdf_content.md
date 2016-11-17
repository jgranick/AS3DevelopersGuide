## Scripting PDF content {#scripting-pdf-content}

Adobe AIR 1.0 and later

You can use JavaScript to control PDF content just as you can in a web page in the browser. JavaScript extensions to Acrobat provide the following features, among others:

• Controlling page navigation and magnification

• Processing forms within the document

• Controlling multimedia events

Full details on JavaScript extensions for Adobe Acrobat are provided at the Adobe Acrobat Developer Connection at [http://www.adobe.com/devnet/acrobat/javascript.html](http://www.adobe.com/devnet/acrobat/javascript.html).

**HTML-PDF communication basics**

Adobe AIR 1.0 and later

JavaScript in an HTML page can send a message to JavaScript in PDF content by calling the postMessage() method of the DOM object representing the PDF content. For example, consider the following embedded PDF content:

&lt;object id=&quot;PDFObj&quot; data=&quot;test.pdf&quot; type=&quot;application/pdf&quot; width=&quot;100%&quot; height=&quot;100%&quot;/&gt;

The following JavaScript code in the containing HTML content sends a message to the JavaScript in the PDF file:

pdfObject = document.getElementById(&quot;PDFObj&quot;); pdfObject.postMessage([&quot;testMsg&quot;, &quot;hello&quot;]);

The PDF file can include JavaScript for receiving this message. You can add JavaScript code to PDF files in some contexts, including the document-, folder-, page-, field-, and batch-level contexts. Only the document-level context, which defines scripts that are evaluated when the PDF document opens, is discussed here.

A PDF file can add a messageHandler property to the hostContainer object. The messageHandler property is an object that defines handler functions to respond to messages. For example, the following code defines the function to handle messages received by the PDF file from the host container (which is the HTML content embedding the PDF file):

this.hostContainer.messageHandler = {onMessage: myOnMessage};

function myOnMessage(aMessage)

{

if(aMessage[0] == &quot;testMsg&quot;)

{

app.alert(&quot;Test message: &quot; + aMessage[1]);

}

else

{

app.alert(&quot;Error&quot;);

}

}

JavaScript code in the HTML page can call the postMessage() method of the PDF object contained in the page. Calling this method sends a message (&quot;Hello from HTML&quot;) to the document-level JavaScript in the PDF file:

&lt;html&gt;

&lt;head&gt;

&lt;title&gt;PDF Test&lt;/title&gt;

&lt;script&gt;

function init()

{

pdfObject = document.getElementById(&quot;PDFObj&quot;); try {

pdfObject.postMessage([&quot;alert&quot;, &quot;Hello from HTML&quot;]);

}

catch (e)

{

alert( &quot;Error: \n name = &quot; + e.name + &quot;\n message = &quot; + e.message );

}

}

&lt;/script&gt;

&lt;/head&gt;

&lt;body onload=&#039;init()&#039;&gt;

&lt;object

id=&quot;PDFObj&quot; data=&quot;test.pdf&quot; type=&quot;application/pdf&quot;

width=&quot;100%&quot; height=&quot;100%&quot;/&gt;

&lt;/body&gt;

&lt;/html&gt;

For a more advanced example, and for information on using Acrobat 8 to add JavaScript to a PDF file, see [Cross-](http://www.adobe.com/go/learn_air_qs_pdf_script_flex_en) [scripting PDF content in Adobe AIR](http://www.adobe.com/go/learn_air_qs_pdf_script_flex_en).

### Scripting PDF content from ActionScript {#scripting-pdf-content-from-actionscript}

Adobe AIR 1.0 and later

ActionScript code (in SWF content) cannot directly communicate with JavaScript in PDF content. However, ActionScript can communicate with the JavaScript in the HTML page loaded in an HTMLLoader object that loads PDF content, and that JavaScript code can communicate with the JavaScript in the loaded PDF file. For more information, see

“Programming HTML and JavaScript in AIR” on page 981

.