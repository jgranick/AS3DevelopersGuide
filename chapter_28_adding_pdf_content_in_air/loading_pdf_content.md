## Loading PDF content {#loading-pdf-content}

Adobe AIR 1.0 and later

You can add a PDF to an AIR application by creating an HTMLLoader instance, setting its dimensions, and loading the path of a PDF.

The following example loads a PDF from an external site. Replace the URLRequest with the path to an available external PDF.

var request:URLRequest = new [URLRequest(&quot;http://www.example.com/test.pdf&quot;);](http://www.example.com/test.pdf) pdf = new HTMLLoader();

pdf.height = 800;

pdf.width = 600; pdf.load(request); container.addChild(pdf);

You can also load content from file URLs and AIR-specific URL schemes, such as app and app-storage. For example, the following code loads the test.pdf file in the PDFs subdirectory of the application directory:

app:/js_api_reference.pdf

For more information on AIR URL schemes, see

“URI schemes” on page 813

.