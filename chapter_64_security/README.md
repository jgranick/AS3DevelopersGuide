# Chapter 64: Security {#chapter-64-security}

Flash Player 9 and later, Adobe AIR 1.0 and later

Security is a key concern of Adobe, users, website owners, and content developers. For this reason, Adobe® Flash® Player and Adobe® AIR™ include a set of security rules and controls to safeguard the user, website owner, and content developer. This discussion covers the security model for SWF files published with ActionScript 3.0 and running in Flash Player 9.0.124.0 or later, and SWF, HTML, and JavaScript files running in AIR 1.0 or later, unless otherwise noted.

This discussion provides an overview of security; it does not try to comprehensively explain all implementation details, usage scenarios, or ramifications for using certain APIs. For a more detailed discussion of Flash Player security concepts, see the Flash Player Developer Center topic “Security” at [www.adobe.com/go/devnet_security_en](http://www.adobe.com/go/devnet_security_en).

**Flash Platform security overview**

Flash Player 9 and later, Adobe AIR 1.0 and later

Much of security model used by the Flash Player and AIR runtimes is based on the domain of origin for loaded SWF files, HTML, media, and other assets. Executable code in a file from a specific Internet domain, such as [www.example.com,](http://www.example.com/) can always access all data from that domain. These assets are put in the same security grouping, known as a _security sandbox_. (For more information, see

“Security sandboxes” on page 1044

.)

For example, ActionScript code in a SWF file can load SWF files, bitmaps, audio, text files, and any other asset from its own domain. Also, cross-scripting between two SWF files from the same domain is always permitted, as long as both files are written using ActionScript 3.0\. _Cross-scripting_ is the ability of code in one file to access the properties, methods, and objects defined by the code in another file.

Cross-scripting is not supported between SWF files written using ActionScript 3.0 and those using previous versions of ActionScript; however, these files can communicate by using the LocalConnection class. Also, the ability of a SWF file to cross-script ActionScript 3.0 SWF files from other domains and to load data from other domains is prohibited by default; however, such access can be granted with a call to the Security.allowDomain() method in the loaded SWF file. For more information, see

“Cross-scripting” on page 1062

.

The following basic security rules always apply by default:

• Resources in the same security sandbox can always access each other.

• Executable code in files in a remote sandbox can never access local files and data.

The Flash Player and AIR runtimes consider the following to be individual domains, and set up individual security sandboxes for each:

• [http://example.com](http://example.com/)

• [http://www.example.com](http://www.example.com/)

• [http://store.example.com](http://store.example.com/)

• [https://www.example.com](http://www.example.com/)

• [http://192.0.34.166](http://192.0.34.166/)

Even if a named domain, such as [http://example.com,](http://example.com/) maps to a specific IP address, such as [http://192.0.34.166,](http://192.0.34.166/) the runtimes set up separate security sandboxes for each.

There are two basic methods that a developer can use to grant a SWF file access to assets from sandboxes other than that of the SWF file:

• The Security.allowDomain() method (see

“Author (developer) controls” on page 1054

)

• The URL policy file (see

“Website controls (policy files)” on page 1051

)

In the Flash Player and AIR runtime security models, there is a distinction between loading content and extracting or accessing data. _Content_ is defined as media, including visual media the runtimes can display, audio, video, or a SWF file or HTML that includes displayed media. _Data_ is defined as something that is accessible only to code. Content and data are loaded in different ways.

• Loading content—You can load content using classes such as the Loader, Sound, and NetStream classes; through MXML tags when using Flex; or through HTML tags in an AIR application.

• Extracting data—You can extract data from loaded media content by using Bitmap objects, the BitmapData.draw() and BitmapData.drawWithQuality() methods, the Sound.id3 property, or the SoundMixer.computeSpectrum() method. The drawWithQuality method is available in Flash Player 11.3 and higher; AIR 3.3 and higher.

• Accessing data—You can access data directly by loading it from an external file (such as an XML file) using classes such as the URLStream, URLLoader, FileReference, Socket, and XMLSocket classes. AIR provides additional classes for loading data, such as FileStream, and XMLHttpRequest.

The Flash Player security model defines different rules for loading content and accessing data. In general, there are fewer restrictions on loading content than on accessing data.

In general, content (SWF files, bitmaps, mp3 files, and videos) can be loaded from anywhere, but if the content is from a domain other than that of the loading code or content, it will be partitioned in a separate security sandbox.

There are a few barriers to loading content:

• By default, local SWF files (those loaded from a non-network address, such as a user’s hard drive) are classified in the local-with-filesystem sandbox. These files cannot load content from the network. For more information, see

“Local sandboxes” on page 1044

.

• Real-Time Messaging Protocol (RTMP) servers can limit access to content. For more information, see

“Content

delivered using RTMP servers” on page 1062

.

If the loaded media is an image, audio, or video, its data, such as pixel data and sound data, can be accessed by a SWF file outside its security sandbox only if the domain of that SWF file has been included in a URL policy file at the origin domain of the media. For details, see

“Accessing loaded media as data” on page 1065

.

Other forms of loaded data include text or XML files, which are loaded with a URLLoader object. Again in this case, to access any data from another security sandbox, permission must be granted by means of a URL policy file at the origin domain. For details, see

“Using URLLoader and URLStream” on page 1068

.

**_Note:_ **_Policy files are never required in order for code executing in the AIR application sandbox to load remote content or data._