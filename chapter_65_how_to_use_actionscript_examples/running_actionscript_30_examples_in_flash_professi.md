## Running ActionScript 3.0 examples in Flash Professional {#running-actionscript-3-0-examples-in-flash-professional}

Use one of the following procedures (depending on example type) to run an example using Flash Professional.

Running a code snippet example in Flash Professional

To run a code snippet example in Flash Professional:

1.  Select File &gt; New.
2.  In the New Document dialog box, select Flash Document, and click OK. A new Flash window is displayed.
3.  Click on the first frame of the first layer in the Timeline panel.
4.  In the Actions panel, type or paste the code snippet example.
5.  Select File &gt; Save. Give the file a name and click OK.
6.  To test the example, select Control &gt; Test Movie.

Running a class-based example in Flash Professional

To run a class-based example in Flash Professional:

1.  Select File &gt; New.
2.  In the New Document dialog box, select ActionScript File, and click OK. A new editor window is displayed.
3.  Copy the class-based example code and paste it into the editor window.

If the class is the main document class for the program, it must extend the MovieClip class:

import flash.display.MovieClip;

public class Example1 extends MovieClip{

//...

}

Also make sure that all the classes referenced in the example are declared using import statements.

1.  Select File &gt; Save. Give the file the same name as the class in the example (e.g. ContextMenuExample.as).

**_Note:_ **_Some of the class-based examples, such as the_ [_flashx.textLayout.container.ContainerController class example_](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flashx/textLayout/container/ContainerController.html#includeExamplesSummary)_, include multiple levels in the package declaration (package flashx.textLayout.container.examples {). For these examples, save the file in a sub folder that matches the package declaration (flashx/textLayout/container/examples), or remove the package name (so the ActionScript starts with package { only) and you can test the file from any location._

1.  Select File &gt; New.
2.  In the New Document dialog box, select Flash Document (ActionScript 3.0), and click OK. A new Flash window is displayed.
3.  In the Properties panel, in the Document Class field, enter the name of the example class, which should match the name of the ActionScript source file you just saved (e.g. ContextMenuExample).
4.  Select File &gt; Save. Give the FLA file the same name as the class in the example (e.g. ContextMenuExample.fla).
5.  To test the example, select Control &gt; Test Movie.

Running a practical example in Flash Professional

Practical examples are normally delivered as ZIP archive files. To run a practical example using Flash Professional:

1.  Unzip the archive file into a folder of your choice.
2.  In Flash Professional select File &gt; Open.
3.  Browse to the folder where you unzipped the archive file. Select the FLA file in that folder and click Open.
4.  To test the example, select Control &gt; Test Movie.