## Accessing AIR API classes from JavaScript {#accessing-air-api-classes-from-javascript}

Adobe AIR 1.0 and later

In addition to the standard and extended elements of Webkit, HTML and JavaScript code can access the host classes provided by the runtime. These classes let you access the advanced features that AIR provides, including:

• Access to the file system

• Use of local SQL databases

• Control of application and window menus

• Access to sockets for networking

• Use of user-defined classes and objects

• Sound capabilities

For example, the AIR file API includes a File class, contained in the flash.filesystem package. You can create a File object in JavaScript as follows:

var myFile = new window.runtime.flash.filesystem.File();

The runtime object is a special JavaScript object, available to HTML content running in AIR in the application sandbox. It lets you access runtime classes from JavaScript. The flash property of the runtime object provides access to the flash package. In turn, the flash.filesystem property of the runtime object provides access to the flash.filesystem package (and this package includes the File class). Packages are a way of organizing classes used in ActionScript.

**_Note:_ **_The runtime property is not automatically added to the window objects of pages loaded in a frame or iframe. However, as long as the child document is in the application sandbox, the child can access the runtime property of the parent._

Because the package structure of the runtime classes would require developers to type long strings of JavaScript code strings to access each class (as in window.runtime.flash.desktop.NativeApplication), the AIR SDK includes an AIRAliases.js file that lets you access runtime classes much more easily (for instance, by simply typing air.NativeApplication).

The AIR API classes are discussed throughout this guide. Other classes from the Flash Player API, which may be of interest to HTML developers, are described in the _Adobe AIR API Reference for HTML Developers_. ActionScript is the language used in SWF (Flash Player) content. However, JavaScript and ActionScript syntax are similar. (They are both based on versions of the ECMAScript language.) All built-in classes are available in both JavaScript (in HTML content) and ActionScript (in SWF content).

**_Note:_ **_JavaScript code cannot use the Dictionary, XML, and XMLList classes, which are available in ActionScript._

**Using the AIRAliases.js file**

Adobe AIR 1.0 and later

The runtime classes are organized in a package structure, as in the following:

• window.runtime.flash.desktop.NativeApplication

• window.runtime.flash.desktop.ClipboardManager

• window.runtime.flash.filesystem.FileStream

• window.runtime.flash.data.SQLDatabase

Included in the AIR SDK is an AIRAliases.js file that provide “alias” definitions that let you access the runtime classes with less typing. For example, you can access the classes listed above by simply typing the following:

• air.NativeApplication

• air.Clipboard

• air.FileStream

• air.SQLDatabase

This list is just a short subset of the classes in the AIRAliases.js file. The complete list of classes and package-level functions is provided in the _Adobe AIR API Reference for HTML Developers_.

In addition to commonly used runtime classes, the AIRAliases.js file includes aliases for commonly used package- level functions: window.runtime.trace(), window.runtime.flash.net.navigateToURL(), and window.runtime.flash.net.sendToURL(), which are aliased as air.trace(), air.navigateToURL(), and air.sendToURL().

To use the AIRAliases.js file, include the following script reference in your HTML page:

&lt;script src=&quot;AIRAliases.js&quot;&gt;&lt;/script&gt;

Adjust the path in the src reference, as needed.

**_Important:_ **_Except where noted, the JavaScript example code in this documentation assumes that you have included the AIRAliases.js file in your HTML page._