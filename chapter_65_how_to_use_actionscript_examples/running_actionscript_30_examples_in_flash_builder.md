## Running ActionScript 3.0 examples in Flash Builder {#running-actionscript-3-0-examples-in-flash-builder}

Use one of the following procedures (depending on example type) to run an example using Flash Builder.

Running a code snippet example in Flash Builder

To run a code snippet example in Flash Builder:

1.  Either create a new Flex Project (select File &gt; New &gt; Flex Project), or within an existing Flex project create a new MXML Application (select File &gt; New &gt; MXML Application). Give the project or application a descriptive name (such as ContextMenuExample).
2.  Inside the generated MXML file, add a &lt;mx:Script&gt; tag.
3.  Paste the contents of the code snippet example between the &lt;mx:Script&gt; and &lt;/mx:Script&gt; tags. Save the MXML file.
4.  To run the example, select the Run &gt; Run menu option for the main MXML file (such as Run &gt; Run ContextMenuExample).

Running a class-based example in Flash Builder

To run a class-based example in Flash Builder:

1.  Select File &gt; New &gt; ActionScript Project.
2.  Enter the name of the primary class (such as ContextMenuExample) into the Project Name field. Use the default values for other fields (or change them according to your specific environment). Click Finish to create the project and the main ActionScript file.
3.  Erase any generated content from the ActionScript file. Paste the example code, including package and import statements, into the ActionScript file and save the file.

**_Note:_ **_Some of the class-based examples, such as the_ [_flashx.textLayout.container.ContainerController class example_](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flashx/textLayout/container/ContainerController.html#includeExamplesSummary)_, include multiple levels in the package declaration (package flashx.textLayout.container.examples {). For these examples, save the file in a sub folder that matches the package declaration (flashx/textLayout/container/examples), or remove the package name (so the ActionScript starts with package { only) and you can test the file from any location._

1.  To run the example, select the Run &gt; Run menu option for the main ActionScript class name (such as Run &gt; Run ContextMenuExample).

Running a practical example inFlash Builder

Practical examples are normally delivered as ZIP archive files. To run a practical example using Flash Builder:

1.  Unzip the archive file into a folder of your choice. Give the folder a descriptive name (such as ContextMenuExample).
2.  In Flash Builder select File &gt; New Flex Project. In the Project Location section, click Browse and select the folder containing the example files. In the Project Name field enter the folder name (such as ContextMenuExample). Use the default values for other fields (or change them according to your specific environment). Click Next to continue.
3.  In the Output panel click Next to accept the default value.
4.  In the Source Paths panel click the Browse button next to the Main Application File field. Select the main MXML example file from the example folder. Click Finish to create the project files.
5.  To run the example, select the Run &gt; Run menu option for the main MXML file (such as Run &gt; Run ContextMenuExample).