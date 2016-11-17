## Running ActionScript 3.0 examples on mobile devices {#running-actionscript-3-0-examples-on-mobile-devices}

You can run the ActionScript 3.0 code examples on mobile devices that support Flash Player 10.1\. However, typically you run a code example to learn how particular classes and methods work. In that case, run the example on a non- mobile device such as a desktop computer. On the desktop computer, you can use trace statements and other debugging tools in Flash Professional or Flash Builder to increase your understanding of a code example.

If you want to run the example on a mobile device, you can either copy the files to the device or to a web server. To copy files to the device and run the example in the browser, do the following:

1.  Create the SWF file by following the instructions in

    “Running ActionScript 3.0 examples in Flash Professional” on

    page 1098

    or in

    “Running ActionScript 3.0 examples in Flash Builder” on page 1099

    . In Flash Professional, you create the SWF file when you select Control &gt; Test Movie. In Flash Builder, you create the SWF file when you run, debug, or build your Flash Builder project.
2.  Copy the SWF file to a directory on the mobile device. Use software provided with the device to copy the file.
3.  In the address bar of browser on the mobile device, enter the file:// URL for the SWF file. For example, enter

file:://applications/myExample.swf.

To copy files to a web server and run the example in the device’s browser, do the following:

1.  Create a SWF file and an HTML file. First, follow the instructions in

    “Running ActionScript 3.0 examples in Flash

    Professional” on page 1098

    or in

    “Running ActionScript 3.0 examples in Flash Builder” on page 1099

    . In Flash Professional, selecting Control &gt; Test Movie creates only the SWF file. To create both files, first select both Flash and HTML on the Formats tab in the Publish Settings dialog. Then select File &gt; Publish to create both the HTML and SWF files. In Flash Builder, you create both the SWF file and HTML file when you run, debug, or build your Flash Builder project.
2.  Copy the SWF file and HTML file to a directory on the web server.
3.  In the address bar of browser on the mobile device, enter the HTTP address for the HTML file. For example, enter

http://www.myWebServer/examples/myExample.html.

Before running an example on a mobile device, consider each of the following issues.

Stage size

The stage size you use when running an example on a mobile device is much smaller than when you use a non-mobile device. Many examples do not require a particular Stage size. When creating the SWF file, specify a Stage size appropriate to your device. For example, specify 176 x 208 pixels.

The purpose of the practical examples in the ActionScript 3.0 Development Guide is to illustrate different ActionScript

3.0 concepts and classes. Their user interfaces are designed to look good and work well on a desktop or laptop computer. Although the examples work on mobile devices, the Stage size and user interface design is not suitable to the small screen. Adobe recommends that you run the practical examples on a computer to learn the ActionScript, and then use pertinent code snippets in your mobile applications.

Text fields instead of trace statements

When running an example on a mobile device, you cannot see the output from the example’s trace statements. To see the output, create an instance of the TextField class. Then, append the text from the trace statements to the text property of the text field.

You can use the following function to set up a text field to use for tracing:

function createTracingTextField(x:Number, y:Number,

width:Number, height:Number):TextField {

var tracingTF:TextField = new TextField(); tracingTF.x = x;

tracingTF.y = y; tracingTF.width = width; tracingTF.height = height;

// A border lets you more easily see the area the text field covers. tracingTF.border = true;

// Left justifying means that the right side of the text field is automatically

// resized if a line of text is wider than the width of the text field.

// The bottom is also automatically resized if the number of lines of text

// exceed the length of the text field. tracingTF.autoSize = TextFieldAutoSize.LEFT;

// Use a text size that works well on the device. var myFormat:TextFormat = new TextFormat(); myFormat.size = 18;

tracingTF.defaultTextFormat = myFormat;

addChild(tracingTF); return tracingTF;

}

For example, add this function to the document class as a private function. Then, in other methods of the document class, trace data with code like the following:

var traceField:TextField = createTracingTextField(10, 10, 150, 150);

// Use the newline character &quot;\n&quot; to force the text to the next line. traceField.appendText(&quot;data to trace\n&quot;);

traceField.appendText(&quot;more data to trace\n&quot;);

// Use the following line to clear the text field. traceField.appendText(&quot;&quot;);

The appendText() method accepts only one value as a parameter. That value is a string (either a String instance or a string literal). To print the value of a non-string variable, first convert the value to a String. The easiest way to do that is to call the object’s toString() method:

var albumYear:int = 1999; traceField.appendText(&quot;albumYear = &quot;); traceField.appendText(albumYear.toString());

Text size

Many examples use text fields to help illustrate a concept. Sometimes adjusting the size of the text in the text field provides better readability on a mobile device. For example, if an example uses a text field instance named myTextField, change the size of its text with the following code:

// Use a text size that works well on the device. var myFormat:TextFormat = new TextFormat(); myFormat.size = 18;

myTextField.defaultTextFormat = myFormat

Capturing user input

The mobile operating system and browser capture some user input events that the SWF content does not receive. Specific behavior depends on the operating system and browser, but could result in unexpected behavior when you run the examples on a mobile device. For more information, see

“KeyboardEvent precedence” on page 562

.

Also, the user interfaces of many examples are designed for a desktop or laptop computer. For example, most of the practical examples in the ActionScript 3.0 Developer’s Guide are well-suited for desktop viewing. Therefore, the entire Stage is sometimes not visible on the mobile device’s screen. The ability to pan through the browser’s contents depends on the browser. Furthermore, the examples are not designed to catch and handle scrolling or panning events.

Therefore, some examples’ user interfaces are not suitable for running on the small screen. Adobe recommends that you run the examples on a computer to learn the ActionScript, and then use pertinent code snippets in your mobile applications.

For more information, see

“Panning and scrolling display objects” on page 178

.

Handling focus

Some examples require you to give a field the focus. By giving a field the focus, you can, for example, enter text or select a button. To give a field focus, use the mobile device’s pointer device, such as a stylus or your finger. Or, use the mobile device’s navigation keys to give a field focus. To select a button that has the focus, use the mobile device’s Select key as you would use Enter on a computer. On some devices, tapping twice on a button selects it.

For more information about focus, see

“Managing focus” on page 557

.

Handling mouse events

Many examples listen for mouse events. On a computer, these mouse events can occur, for example, when a user rolls over a display object with the mouse, or clicks the mouse button on a display object. On mobile devices, events from using pointer devices such as a stylus or finger, are called touch events. Flash Player 10.1 maps touch events to mouse events. This mapping ensures that SWF content that was developed before Flash Player 10.1 continues to work.

Therefore, examples work when using a pointer device to select or drag a display object.

Performance

Mobile devices have less processing power than desktop devices. Some CPU-intensive examples possibly perform slowly on mobile devices. For example, the example in

“Drawing API example: Algorithmic Visual Generator” on

page 233

does extensive computations and drawing upon entering every frame. Running this example on a computer illustrates various drawing APIs. However, the example is not suitable on some mobile devices due to their performance limitations.

For more information about performance on mobile devices, see [_Optimizing Performance for the Flash Platform_](http://www.adobe.com/go/learn_optimizing_fp_en).

Best practices

The examples do not consider best practices in developing applications for mobile devices. Limitations in memory and processing power in mobile devices require special consideration. Similarly, the user interface for the small screen has different needs than a desktop display. For more information about developing applications for mobile devices, _see_ [_Optimizing Performance for the Flash Platform_](http://www.adobe.com/go/learn_optimizing_fp_en)_._