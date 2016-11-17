## Importing and exporting text with TLF {#importing-and-exporting-text-with-tlf}

Flash Player 10 and later, Adobe AIR 1.5 and later

The TextConverter class in the flashx.textLayout.conversion.* package lets you import text to, and export text from, the TLF. Use this class if you plan to load text at runtime instead of compiling the text into the SWF file. You can also use this class to export text that is stored in a TextFlow instance into a String or XML object.

Both import and export are straightforward procedures. You call either the export() method or the importToFlow() method, both of which are part of the TextConverter class. Both methods are static, which means that you call the methods on the TextConverter class rather than on an instance of the TextConverter class.

The classes in the flashx.textLayout.conversion package provide considerable flexibility in where you choose to store your text. For example, if you store your text in a database, you can import the text into the framework for display. You can then use the classes in the flashx.textLayout.edit package to change the text, and export the changed text back to your database.

For more information, see flashx.textLayout.conversion in the ActionScript 3.0 Reference for the Adobe Flash Platform.