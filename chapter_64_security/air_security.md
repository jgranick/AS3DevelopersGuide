## AIR security {#air-security}

Adobe AIR 1.0 and later

**AIR security basics**

**Adobe AIR 1.0 and later**

AIR applications run with the same security restrictions as native applications. In general, AIR applications, like native applications, have broad access to operating system capabilities such as reading and writing files, starting applications, drawing to the screen, and communicating with the network. Operating system restrictions that apply to native applications, such as user-specific privileges, equally apply to AIR applications.

Although the Adobe® AIR® security model is an evolution of the Adobe® Flash® Player security model, the security contract is different from the security contract applied to content in a browser. This contract offers developers a secure means of broader functionality for rich experiences with freedoms that would be inappropriate for a browser-based application.

AIR applications are written using either compiled bytecode (SWF content) or interpreted script (JavaScript, HTML) so that the runtime provides memory management. This minimizes the chances of AIR applications being affected by vulnerabilities related to memory management, such as buffer overflows and memory corruption. These are some of the most common vulnerabilities affecting desktop applications written in native code.

**Installation and updates**

Adobe AIR 1.0 and later

AIR applications are distributed via AIR installer files which use the air extension or via native installers, which use the file format and extension of the native platform. For example, the native installer format of Windows is an EXE file, and for Android the native format is an APK file.

When Adobe AIR is installed and an AIR installer file is opened, the AIR runtime administers the installation process. When a native installer is used, the operating system administers the installation process.

**_Note:_ **_Developers can specify a version, and application name, and a publisher source when using the AIR file format, but the initial application installation workflow itself cannot be modified. This restriction is advantageous for users because all AIR applications share a secure, streamlined, and consistent installation procedure administered by the runtime. If application customization is necessary, it can be provided when the application is first executed._

**Runtime installation location**

Adobe AIR 1.0 and later

AIR applications using the AIR file format first require the runtime to be installed on a user&#039;s computer, just as SWF files first require the Flash Player browser plug-in to be installed.

The runtime is installed to the following location on desktop computers:

• Mac OS: /Library/Frameworks/

• Windows: C:\Program Files\Common Files\Adobe AIR

• Linux: /opt/Adobe AIR/

On Mac OS, to install an updated version of an application, the user must have adequate system privileges to install to the application directory. On Windows and Linux, a user must have administrative privileges.

**_Note:_ **_On iOS, the AIR runtime is not installed separately; every AIR app is a self-contained application._

The runtime can be installed in two ways: using the seamless install feature (installing directly from a web browser) or via a manual install. AIR applications packaged as native installers can also install the AIR runtime as part of their normal application install process. (Distributing the AIR runtime in this way requires a redistribution agreement with Adobe.)

Seamless install (runtime and application)

Adobe AIR 1.0 and later

The seamless install feature provides developers with a streamlined installation experience for users who do not have Adobe AIR installed yet. In the seamless install method, the developer creates a SWF file that presents the application for installation. When a user clicks in the SWF file to install the application, the SWF file attempts to detect the runtime. If the runtime cannot be detected it is installed, and the runtime is activated immediately with the installation process for the developer&#039;s application.

Manual install

Adobe AIR 1.0 and later

Alternatively, the user can manually download and install the runtime before opening an AIR file. The developer can then distribute an AIR file by different means (for instance, via e-mail or an HTML link on a website). When the AIR file is opened, the runtime begins to process the application installation.

Application installation flow

Adobe AIR 1.0 and later

The AIR security model allows users to decide whether to install an AIR application. The AIR install experience provides several improvements over native application install technologies that make this trust decision easier for users:

• The runtime provides a consistent installation experience on all operating systems, even when an AIR application is installed from a link in a web browser. Most native application install experiences depend upon the browser or other application to provide security information, if it is provided at all.

• The AIR application install experience identifies the source of the application and information about what privileges are available to the application (if the user allows the installation to proceed).

• The runtime administers the installation process of an AIR application. An AIR application cannot manipulate the installation process the runtime uses.

In general, users should not install any desktop application that comes from a source that they do not trust, or that cannot be verified. The burden of proof on security for native applications is equally true for AIR applications as it is for other installable applications.

Application destination

Adobe AIR 1.0 and later

The installation directory can be set using one of the following two options:

1.  The user customizes the destination during installation. The application installs to wherever the user specifies.
2.  If the user does not change the install destination, the application installs to the default path as determined by the runtime:
    *   Mac OS: ~/Applications/
    *   Windows XP and earlier: C:\Program Files\
    *   Windows Vista: ~/Apps/
    *   Linux: /opt/

If the developer specifies an installFolder setting in the application descriptor file, the application is installed to a subpath of this directory.

The AIR file system

Adobe AIR 1.0 and later

The install process for AIR applications copies all files that the developer has included within the AIR installer file onto the user&#039;s local computer. The installed application is composed of:

• Windows: A directory containing all files included in the AIR installer file. The runtime also creates an exe file during the installation of the AIR application.

• Linux: A directory containing all files included in the AIR installer file. The runtime also creates a bin file during the installation of the AIR application.

• Mac OS: An app file that contains all of the contents of the AIR installer file. It can be inspected using the &quot;Show Package Contents&quot; option in Finder. The runtime creates this app file as part of the installation of the AIR application.

An AIR application is run by:

• Windows: Running the .exe file in the install folder, or a shortcut that corresponds to this file (such as a shortcut on the Start Menu or desktop).

• Linux: Launching the .bin file in the install folder, choosing the application from the Applications menu, or running from an alias or desktop shortcut.

• Mac OS: Running the .app file or an alias that points to it.

The application file system also includes subdirectories related to the function of the application. For example, information written to encrypted local storage is saved to a subdirectory in a directory named after the application identifier of the application.

AIR application storage

Adobe AIR 1.0 and later

AIR applications have privileges to write to any location on the user&#039;s hard drive; however, developers are encouraged to use the app-storage:/ path for local storage related to their application. Files written to app-storage:/ from an application are located in a standard location depending on the user&#039;s operating system:

• On Mac OS: the storage directory of an application varies by AIR version:

• **AIR 3.2 and earlier** - &lt;appData&gt;/&lt;appId&gt;/Local Store/ where &lt;appData&gt; is the user&#039;s “preferences folder,” typically: /Users/&lt;user&gt;/Library/Preferences

• **AIR 3.3 and higher** - &lt;path&gt;/Library/Application Support/&lt;appID&gt;/Local Store, where &lt;path&gt; is either /Users/&lt;user&gt;/Library/Containers/&lt;bundle-id&gt;/Data (sandboxed environment) or

/Users/&lt;user&gt; ( when running outside a sandboxed environment)

• On Windows: the storage directory of an application is &lt;appData&gt;\&lt;appId&gt;\Local Store\ where &lt;appData&gt; is the user&#039;s CSIDL_APPDATA “Special Folder,” typically: C:\Documents and Settings\&lt;user&gt;\Application Data

• On Linux: &lt;appData&gt;/&lt;appID&gt;/Local Store/where &lt;appData&gt; is /home/&lt;user&gt;/.appdata

You can access the application storage directory via the air.File.applicationStorageDirectory property. You can access its contents using the resolvePath() method of the File class. For details, see

“Working with the file

system” on page 653

.

Updating Adobe AIR

Adobe AIR 1.0 and later

When the user installs an AIR application that requires an updated version of the runtime, the runtime automatically installs the required runtime update.

To update the runtime, a user must have administrative privileges for the computer.

Updating AIR applications

Adobe AIR 1.0 and later

Development and deployment of software updates are one of the biggest security challenges facing native code applications. The AIR API provides a mechanism to improve this: the Updater.update() method can be invoked upon launch to check a remote location for an AIR file. If an update is appropriate, the AIR file is downloaded, installed, and the application restarts. Developers can use this class not only to provide new functionality but also respond to potential security vulnerabilities.

The Updater class can only be used to update applications distributed as AIR files. Applications distributed as native applications must use the update facilities, if any, of the native operating system.

**_Note:_ **_Developers can specify the version of an application by setting the versionNumber property of the application descriptor file._

Uninstalling an AIR application

Adobe AIR 1.0 and later

Removing an AIR application removes all files in the application directory. However, it does not remove all files that the application may have written to outside of the application directory. Removing AIR applications does not revert changes the AIR application has made to files outside of the application directory.

Windows registry settings for administrators

Adobe AIR 1.0 and later

On Windows, administrators can configure a machine to prevent (or allow) AIR application installation and runtime updates. These settings are contained in the Windows registry under the following key: HKLM\Software\Policies\Adobe\AIR. They include the following:

| **Registry setting** | **Description** |
| --- | --- |
| AppInstallDisabled | Specifies that AIR application installation and uninstallation are allowed. Set to 0 for “allowed,” set to 1 for “disallowed.” |
| UntrustedAppInstallDisabled | Specifies that installation of untrusted AIR applications (applications that do not includes a trusted certificate) is allowed. Set to 0 for “allowed,” set to 1 for “disallowed.” |
| UpdateDisabled | Specifies that updating the runtime is allowed, either as a background task or as part of an explicit installation. Set to 0 for “allowed,” set to 1 for “disallowed.” |

### HTML security in Adobe AIR {#html-security-in-adobe-air}

Adobe AIR 1.0 and later

This topic describes the AIR HTML security architecture and how to use iframes, frames, and the sandbox bridge to set up HTML-based applications and safely integrate HTML content into SWF-based applications.

The runtime enforces rules and provides mechanisms for overcoming possible security vulnerabilities in HTML and JavaScript. The same rules are enforced whether your application is primarily written in JavaScript or whether you load the HTML and JavaScript content into a SWF-based application. Content in the application sandbox and the non- application security sandbox have different privileges. When loading content into an iframe or frame, the runtime provides a secure _sandbox bridge_ mechanism that allows content in the frame or iframe to communicate securely with content in the application security sandbox.

The AIR SDK provides three classes for rendering HTML content.

The HTMLLoader class provides close integration between JavaScript code and the AIR APIs.

The StageWebView class is an HTML rendering class and has very limited integration with the host AIR application. Content loaded by the StageWebView class is never placed in the application security sandbox and cannot access data or call functions in the host AIR application. On desktop platforms, the StageWebView class uses the built-in AIR HTML engine, based on Webkit, which is also used by the HTMLLoader class. On mobile platforms, the StageWebView class uses the HTML control provided by the operating system. Thus, on mobile platforms the StageWebView class has the same security considerations and vulnerabilities as the system web browser.

The TextField class can display strings of HTML text. No JavaScript can be executed, but the text can include links and externally loaded images.

For more information, see

“Avoiding security-related JavaScript errors” on page 983

.

**Overview on configuring your HTML-based application**

Adobe AIR 1.0 and later

Frames and iframes provide a convenient structure for organizing HTML content in AIR. Frames provide a means both for maintaining data persistence and for working securely with remote content.

Because HTML in AIR retains its normal, page-based organization, the HTML environment completely refreshes if the top frame of your HTML content “navigates” to a different page. You can use frames and iframes to maintain data persistence in AIR, much the same as you would for a web application running in a browser. Define your main application objects in the top frame and they persist as long as you don’t allow the frame to navigate to a new page. Use child frames or iframes to load and display the transient parts of the application. (There are a variety of ways to maintain data persistence that can be used in addition to, or instead of, frames. These include cookies, local shared objects, local file storage, the encrypted file store, and local database storage.)

Because HTML in AIR retains its normal, blurred line between executable code and data, AIR puts content in the top frame of the HTML environment into the application sandbox. After the page load event, AIR restricts any operations, such as eval(), that can convert a string of text into an executable object. This restriction is enforced even when an application does not load remote content. To allow HTML content to execute these restricted operations, you must use frames or iframes to place the content into a non-application sandbox. (Running content in a sandboxed child frame may be necessary when using some JavaScript application frameworks that rely on the eval() function.) For a complete list of the restrictions on JavaScript in the application sandbox, see

“Code restrictions for content in

different sandboxes” on page 1082

.

Because HTML in AIR retains its ability to load remote, possibly insecure content, AIR enforces a same-origin policy that prevents content in one domain from interacting with content in another. To allow interaction between application content and content in another domain, you can set up a bridge to serve as the interface between a parent and a child frame.

Setting up a parent-child sandbox relationship

**Adobe AIR 1.0 and later**

AIR adds the sandboxRoot and documentRoot attributes to the HTML frame and iframe elements. These attributes let you treat application content as if it came from another domain:

| **Attribute** | **Description** |
| --- | --- |
| sandboxRoot | The URL to use for determining the sandbox and domain in which to place the frame content. The file:, http:, or https: URL schemes must be used. |
| documentRoot | The URL from which to load the frame content. The file:, app:, or app- storage: URL schemes must be used. |

The following example maps content installed in the sandbox subdirectory of the application to run in the remote sandbox and the [www.example.com](http://www.example.com/) domain:

&lt;iframe

src=&quot;ui.html&quot; [sandboxRoot=&quot;http://www.example.com/local/&quot;](http://www.example.com/local/) documentRoot=&quot;app:/sandbox/&quot;&gt;

&lt;/iframe&gt;

Setting up a bridge between parent and child frames in different sandboxes or domains

**Adobe AIR 1.0 and later**

AIR adds the childSandboxBridge and parentSandboxBridge properties to the window object of any child frame. These properties let you define bridges to serve as interfaces between a parent and a child frame. Each bridge goes in one direction:

childSandboxBridge — The childSandboxBridge property allows the child frame to expose an interface to content in the parent frame. To expose an interface, you set the childSandbox property to a function or object in the child frame. You can then access the object or function from content in the parent frame. The following example shows how a script running in a child frame can expose an object containing a function and a property to its parent:

var interface = {}; interface.calculatePrice = function(){

return .45 + 1.20;

}

interface.storeID = &quot;abc&quot; window.childSandboxBridge = interface;

If this child content is in an iframe assigned an id of &quot;child&quot;, you can access the interface from parent content by reading the childSandboxBridge property of the frame:

var childInterface = document.getElementById(&quot;child&quot;).childSandboxBridge; air.trace(childInterface.calculatePrice()); //traces &quot;1.65&quot; air.trace(childInterface.storeID)); //traces &quot;abc&quot;

parentSandboxBridge — The parentSandboxBridge property allows the parent frame to expose an interface to content in the child frame. To expose an interface, you set the parentSandbox property of the child frame to a function or object in the parent frame. You can then access the object or function from content in the child frame. The following example shows how a script running in the parent frame can expose an object containing a save function to a child:

var interface = {}; interface.save = function(text){

var saveFile = air.File(&quot;app-storage:/save.txt&quot;);

//write text to file

}

document.getElementById(&quot;child&quot;).parentSandboxBridge = interface;

Using this interface, content in the child frame could save text to a file named save.txt. However, it would not have any other access to the file system. In general, application content should expose the narrowest possible interface to other sandboxes. The child content could call the save function as follows:

var textToSave = &quot;A string.&quot;; window.parentSandboxBridge.save(textToSave);

If child content attempts to set a property of the parentSandboxBridge object, the runtime throws a SecurityError exception. If parent content attempts to set a property of the childSandboxBridge object, the runtime throws a SecurityError exception.

Code restrictions for content in different sandboxes

Adobe AIR 1.0 and later

As discussed in the introduction to this topic,

“HTML security in Adobe AIR” on page 1080

, the runtime enforces rules and provides mechanisms for overcoming possible security vulnerabilities in HTML and JavaScript. This topic lists those restrictions. If code attempts to call these restricted APIs, the runtime throws an error with the message “Adobe AIR runtime security violation for JavaScript code in the application security sandbox.”

For more information, see

“Avoiding security-related JavaScript errors” on page 983

.

Restrictions on using the JavaScript eval() function and similar techniques

**Adobe AIR 1.0 and later**

For HTML content in the application security sandbox, there are limitations on using APIs that can dynamically transform strings into executable code after the code is loaded (after the onload event of the body element has been dispatched and the onload handler function has finished executing). This is to prevent the application from inadvertently injecting (and executing) code from non-application sources (such as potentially insecure network domains).

For example, if your application uses string data from a remote source to write to the innerHTML property of a DOM element, the string could include executable (JavaScript) code that could perform insecure operations. However, while the content is loading, there is no risk of inserting remote strings into the DOM.

One restriction is in the use of the JavaScript eval() function. Once code in the application sandbox is loaded and after processing of the onload event handler, you can only use the eval() function in limited ways. The following rules apply to the use of the eval() function _after_ code is loaded from the application security sandbox:

• Expressions involving literals are allowed. For example:

eval(&quot;null&quot;);

eval(&quot;3 + .14&quot;);

eval(&quot;&#039;foo&#039;&quot;);

• Object literals are allowed, as in the following:

{ prop1: val1, prop2: val2 }

• Object literal setter/getters are _prohibited_, as in the following:

{ get prop1() { ... }, set prop1(v) { ... } }

• Array literals are allowed, as in the following:

[ val1, val2, val3 ]

• Expressions involving property reads are _prohibited_, as in the following:

a.b.c

*   Function invocation is _prohibited._
*   Function definitions are _prohibited._
*   Setting any property is _prohibited._
*   Function literals are _prohibited_.

However, while the code is loading, before the onload event, and during execution the onload event handler function, these restrictions do not apply to content in the application security sandbox.

For example, after code is loaded, the following code results in the runtime throwing an exception:

eval(&quot;alert(44)&quot;);

eval(&quot;myFunction(44)&quot;); eval(&quot;NativeApplication.applicationID&quot;);

Dynamically generated code, such as that which is made when calling the eval() function, would pose a security risk if allowed within the application sandbox. For example, an application may inadvertently execute a string loaded from a network domain, and that string may contain malicious code. For example, this could be code to delete or alter files on the user’s computer. Or it could be code that reports back the contents of a local file to an untrusted network domain.

Ways to generate dynamic code are the following:

*   Calling the eval() function.
*   Using innerHTML properties or DOM functions to insert script tags that load a script outside of the application directory.
*   Using innerHTML properties or DOM functions to insert script tags that have inline code (rather than loading a script via the src attribute).
*   Setting the src attribute for a script tags to load a JavaScript file that is outside of the application directory.
*   Using the javascript URL scheme (as in href=&quot;javascript:alert(&#039;Test&#039;)&quot;).
*   Using the setInterval() or setTimout()function where the first parameter (defining the function to run asynchronously) is a string (to be evaluated) rather than a function name (as in setTimeout(&#039;x = 4&#039;, 1000)).
*   Calling document.write() or document.writeln().

Code in the application security sandbox can only use these methods while content is loading.

These restrictions do _not_ prevent using eval() with JSON object literals. This lets your application content work with the JSON JavaScript library. However, you are restricted from using overloaded JSON code (with event handlers).

For other Ajax frameworks and JavaScript code libraries, check to see if the code in the framework or library works within these restrictions on dynamically generated code. If they do not, include any content that uses the framework or library in a non-application security sandbox. For details, see

“Restrictions for JavaScript inside AIR” on page 1046

and

“Scripting between application and non-application content” on page 1090

. Adobe maintains a list of Ajax frameworks known to support the application security sandbox, at [http://www.adobe.com/products/air/develop/ajax/features/](http://www.adobe.com/products/air/develop/ajax/features/).

Unlike content in the application security sandbox, JavaScript content in a non-application security sandbox _can_ call the eval() function to execute dynamically generated code at any time.

Restrictions on access to AIR APIs (for non-application sandboxes)

**Adobe AIR 1.0 and later**

JavaScript code in a non-application sandbox does not have access to the window.runtime object, and as such this code cannot execute AIR APIs. If content in a non-application security sandbox calls the following code, the application throws a TypeError exception:

try {

window.runtime.flash.system.NativeApplication.nativeApplication.exit();

}

catch (e)

{

alert(e);

}

The exception type is TypeError (undefined value), because content in the non-application sandbox does not recognize the window.runtime object, so it is seen as an undefined value.

You can expose runtime functionality to content in a non-application sandbox by using a script bridge. For details, see and

“Scripting between application and non-application content” on page 1090

.

Restrictions on using XMLHttpRequest calls

**Adobe AIR 1.0 and later**

HTML content in the application security sandbox cannot use synchronous XMLHttpRequest methods to load data from outside of the application sandbox while the HTML content is loading and during onLoad event.

By default, HTML content in non-application security sandboxes are not allowed to use the JavaScript XMLHttpRequest object to load data from domains other than the domain calling the request. A frame or iframe tag can include an allowcrosscomainxhr attribute. Setting this attribute to any non-null value allows the content in the frame or iframe to use the JavaScript XMLHttpRequest object to load data from domains other than the domain of the code calling the request:

&lt;iframe id=&quot;UI&quot; [src=&quot;http://example.com/ui.html&quot;](http://example.com/ui.html) [sandboxRoot=&quot;http://example.com/&quot;](http://example.com/) **allowcrossDomainxhr=&quot;true&quot;** documentRoot=&quot;app:/&quot;&gt;

&lt;/iframe&gt;

For more information, see

“Scripting between content in different domains” on page 1086

.

Restrictions on loading CSS, frame, iframe, and img elements (for content in non-application sandboxes)

**Adobe AIR 1.0 and later**

HTML content in remote (network) security sandboxes can only load CSS, frame, iframe, and img content from remote sandboxes (from network URLs).

HTML content in local-with-filesystem, local-with-networking, or local-trusted sandboxes can only load CSS, frame, iframe, and img content from local sandboxes (not from application or remote sandboxes).

Restrictions on calling the JavaScript window.open() method

**Adobe AIR 1.0 and later**

If a window that is created via a call to the JavaScript window.open() method displays content from a non-application security sandbox, the window’s title begins with the title of the main (launching) window, followed by a colon character. You cannot use code to move that portion of the title of the window off screen.

Content in non-application security sandboxes can only successfully call the JavaScript window.open() method in response to an event triggered by a user mouse or keyboard interaction. This prevents non-application content from creating windows that might be used deceptively (for example, for phishing attacks). Also, the event handler for the mouse or keyboard event cannot set the window.open() method to execute after a delay (for example by calling the setTimeout() function).

Content in remote (network) sandboxes can only use the window.open() method to open content in remote network sandboxes. It cannot use the window.open() method to open content from the application or local sandboxes.

Content in the local-with-filesystem, local-with-networking, or local-trusted sandboxes (see

“Security sandboxes” on

page 1044

) can only use the window.open() method to open content in local sandboxes. It cannot use window.open()to open content from the application or remote sandboxes.

Errors when calling restricted code

**Adobe AIR 1.0 and later**

If you call code that is restricted from use in a sandbox due to these security restrictions, the runtime dispatches a JavaScript error: &quot;Adobe AIR runtime security violation for JavaScript code in the application security sandbox.&quot;

For more information, see

“Avoiding security-related JavaScript errors” on page 983

.

Sandbox protection when loading HTML content from a string

Adobe AIR 1.0 and later

The loadString() method of the HTMLLoader class lets you create HTML content at run time. However, data that you use as the HTML content can be corrupted if the data is loaded from an insecure Internet source. For this reason, by default, HTML created using the loadString() method is not placed in the application sandbox and it has no access to AIR APIs. However, you can set the placeLoadStringContentInApplicationSandbox property of an HTMLLoader object to true to place HTML created using the loadString() method into the application sandbox. For more information, see

“Loading HTML content from a string” on page 982

.

### Scripting between content in different domains {#scripting-between-content-in-different-domains}

Adobe AIR 1.0 and later

AIR applications are granted special privileges when they are installed. It is crucial that the same privileges not be leaked to other content, including remote files and local files that are not part of the application.

**About the AIR sandbox bridge**

Adobe AIR 1.0 and later

Normally, content from other domains cannot call scripts in other domains. To protect AIR applications from accidental leakage of privileged information or control, the following restrictions are placed on content in the application security sandbox (content installed with the application):

*   Code in the application security sandbox cannot allow to other sandboxes by calling the Security.allowDomain() method. Calling this method from the application security sandbox will throw an error.
*   Importing non-application content into the application sandbox by setting the LoaderContext.securityDomain

or the LoaderContext.applicationDomain property is prevented.

There are still cases where the main AIR application requires content from a remote domain to have controlled access to scripts in the main AIR application, or vice versa. To accomplish this, the runtime provides a _sandbox bridge_ mechanism, which serves as a gateway between the two sandboxes. A sandbox bridge can provide explicit interaction between remote and application security sandboxes.

The sandbox bridge exposes two objects that both loaded and loading scripts can access:

*   The parentSandboxBridge object lets loading content expose properties and functions to scripts in the loaded content.
*   The childSandboxBridge object lets loaded content expose properties and function to scripts in the loading content.

Objects exposed via the sandbox bridge are passed by value, not by reference. All data is serialized. This means that the objects exposed by one side of the bridge cannot be set by the other side, and that objects exposed are all untyped. Also, you can only expose simple objects and functions; you cannot expose complex objects.

If child content attempts to set an object to the parentSandboxBridge object, the runtime throws a SecurityError exception. Similarly, if parent content attempts to set an object to the childSandboxBridge object, the runtime throws a SecurityError exception.

Sandbox bridge example (SWF)

Adobe AIR 1.0 and later

Suppose an AIR music store application wants to allow remote SWF files to broadcast the price of albums, but does not want the remote SWF file to disclose whether the price is a sale price. To do this, a StoreAPI class provides a method to acquire the price, but obscures the sale price. An instance of this StoreAPI class is then assigned to the parentSandboxBridge property of the LoaderInfo object of the Loader object that loads the remote SWF.

The following is the code for the AIR music store:

&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;

&lt;mx:WindowedApplication [xmlns:mx=&quot;http://www.adobe.com/2006/mxml&quot;](http://www.adobe.com/2006/mxml) layout=&quot;absolute&quot; title=&quot;Music Store&quot; creationComplete=&quot;initApp()&quot;&gt;

&lt;mx:Script&gt;

import flash.display.Loader; import flash.net.URLRequest;

private var child:Loader;

private var isSale:Boolean = false;

private function initApp():void { var request:URLRequest =

new [URLRequest(&quot;http://[www.yourdomain.com]/PriceQuoter.swf&quot;)](http://www.yourdomain.com/)

child = new Loader();

child.contentLoaderInfo.parentSandboxBridge = new StoreAPI(this); child.load(request);

container.addChild(child);

}

public function getRegularAlbumPrice():String { return &quot;$11.99&quot;;

}

public function getSaleAlbumPrice():String { return &quot;$9.99&quot;;

}

public function getAlbumPrice():String { if(isSale) {

return getSaleAlbumPrice();

}

else {

return getRegularAlbumPrice();

}

}

&lt;/mx:Script&gt;

&lt;mx:UIComponent id=&quot;container&quot; /&gt;

&lt;/mx:WindowedApplication&gt;

The StoreAPI object calls the main application to retrieve the regular album price, but returns “Not available” when the getSaleAlbumPrice() method is called. The following code defines the StoreAPI class:

public class StoreAPI

{

private static var musicStore:Object;

public function StoreAPI(musicStore:Object)

{

this.musicStore = musicStore;

}

public function getRegularAlbumPrice():String { return musicStore.getRegularAlbumPrice();

}

public function getSaleAlbumPrice():String { return &quot;Not available&quot;;

}

public function getAlbumPrice():String { return musicStore.getRegularAlbumPrice();

}

}

The following code represents an example of a PriceQuoter SWF file that reports the store’s price, but cannot report the sale price:

package

{

import flash.display.Sprite; import flash.system.Security; import flash.text.*;

public class PriceQuoter extends Sprite

{

private var storeRequester:Object;

public function PriceQuoter() { trace(&quot;Initializing child SWF&quot;);

trace(&quot;Child sandbox: &quot; + Security.sandboxType); storeRequester = loaderInfo.parentSandboxBridge;

var tf:TextField = new TextField(); tf.autoSize = TextFieldAutoSize.LEFT; addChild(tf);

tf.appendText(&quot;Store price of album is: &quot; + storeRequester.getAlbumPrice()); tf.appendText(&quot;\n&quot;);

tf.appendText(&quot;Sale price of album is: &quot; + storeRequester.getSaleAlbumPrice());

}

}

}

Sandbox bridge example (HTML)

Adobe AIR 1.0 and later

In HTML content, the parentSandboxBridge and childSandboxBridge properties are added to the JavaScript window object of a child document. For an example of how to set up bridge functions in HTML content, see

“Setting

up a sandbox bridge interface” on page 1000

.

Limiting API exposure

Adobe AIR 1.0 and later

When exposing sandbox bridges, it&#039;s important to expose high-level APIs that limit the degree to which they can be abused. Keep in mind that the content calling your bridge implementation may be compromised (for example, via a code injection). So, for example, exposing a readFile(path:String) method (that reads the contents of an arbitrary file) via a bridge is vulnerable to abuse. It would be better to expose a readApplicationSetting() API that doesn&#039;t take a path and reads a specific file. The more semantic approach limits the damage that an application can do once part of it is compromised.

**More Help topics**

“Cross-scripting content in different security sandboxes” on page 999

“The AIR application sandbox” on page 1045

### Writing to disk {#writing-to-disk}

Adobe AIR 1.0 and later

Applications running in a web browser have only limited interaction with the user&#039;s local file system. Web browsers implement security policies that ensure that a user&#039;s computer cannot be compromised as a result of loading web content. For example, SWF files running through Flash Player in a browser cannot directly interact with files already on a user&#039;s computer. Shared objects and cookies can be written to a user&#039;s computer for the purpose of maintaining user preferences and other data, but this is the limit of file system interaction. Because AIR applications are natively installed, they have a different security contract, one which includes the capability to read and write across the local file system.

This freedom comes with high responsibility for developers. Accidental application insecurities jeopardize not only the functionality of the application, but also the integrity of the user&#039;s computer. For this reason, developers should read

“Best security practices for developers” on page 1091

.

AIR developers can access and write files to the local file system using several URL scheme conventions:

| **URL scheme** | **Description** |
| --- | --- |
| app:/ | An alias to the application directory. Files accessed from this path are assigned the application sandbox and have the full privileges granted by the runtime. |
| app-storage:/ | An alias to the local storage directory, standardized by the runtime. Files accessed from this path are assigned a non-application sandbox. |
| file:/// | An alias that represents the root of the user&#039;s hard disk. A file accessed from this path is assigned an application sandbox if the file exists in the application directory, and a non-application sandbox otherwise. |

**_Note:_ **_AIR applications cannot modify content using the app: URL scheme. Also, the application directory may be read only because of administrator settings._

Unless there are administrator restrictions to the user&#039;s computer, AIR applications are privileged to write to any location on the user&#039;s hard drive. Developers are advised to use the app-storage:/ path for local storage related to their application. Files written to app-storage:/ from an application are put in a standard location:

*   On Mac OS: the storage directory of an application is &lt;appData&gt;/&lt;appId&gt;/Local Store/ where &lt;appData&gt; is the user&#039;s preferences folder. This is typically /Users/&lt;user&gt;/Library/Preferences
*   On Windows: the storage directory of an application is &lt;appData&gt;\&lt;appId&gt;\Local Store\ where &lt;appData&gt; is the user&#039;s CSIDL_APPDATA Special Folder. This is typically C:\Documents and Settings\&lt;userName&gt;\Application Data
*   On Linux: &lt;appData&gt;/&lt;appID&gt;/Local Store/where &lt;appData&gt; is /home/&lt;user&gt;/.appdata

If an application is designed to interact with existing files in the user&#039;s file system, be sure to read

“Best security

practices for developers” on page 1091

.

### Working securely with untrusted content {#working-securely-with-untrusted-content}

Adobe AIR 1.0 and later

Content not assigned to the application sandbox can provide additional scripting functionality to your application, but only if it meets the security criteria of the runtime. This topic explains the AIR security contract with non-application content.

**Security.allowDomain()**

Adobe AIR 1.0 and later

AIR applications restrict scripting access for non-application content more stringently than the Flash Player browser plug-in restricts scripting access for untrusted content. For example, in Flash Player in the browser, when a SWF file that is assigned to the local-trusted sandbox calls the System.allowDomain() method, scripting access is granted to any SWF loaded from the specified domain. The analogous approach is not permitted from application content in AIR applications, since it would grant unreasonable access unto the non-application file into the user&#039;s file system. Remote files cannot directly access the application sandbox, regardless of calls to the Security.allowDomain() method.

Scripting between application and non-application content

Adobe AIR 1.0 and later

AIR applications that script between application and non-application content have more complex security arrangements. Files that are not in the application sandbox are only allowed to access the properties and methods of files in the application sandbox through the use of a sandbox bridge. A sandbox bridge acts as a gateway between application content and non-application content, providing explicit interaction between the two files. When used correctly, sandbox bridges provide an extra layer of security, restricting non-application content from accessing object references that are part of application content.

The benefit of sandbox bridges is best illustrated through example. Suppose an AIR music store application wants to provide an API to advertisers who want to create their own SWF files, with which the store application can then communicate. The store wants to provide advertisers with methods to look up artists and CDs from the store, but also wants to isolate some methods and properties from the third-party SWF file for security reasons.

A sandbox bridge can provide this functionality. By default, content loaded externally into an AIR application at runtime does not have access to any methods or properties in the main application. With a custom sandbox bridge implementation, a developer can provide services to the remote content without exposing these methods or properties. Consider the sandbox bridge as a pathway between trusted and untrusted content, providing communication between loader and loadee content without exposing object references.

For more information on how to securely use sandbox bridges, see

“Scripting between content in different domains”

on page 1086

.

Protection against dynamically generating unsafe SWF content

Adobe AIR 1.0 and later

The Loader.loadBytes() method provides a way for an application to generate SWF content from a byte array. However, injection attacks on data loaded from remote sources could do severe damage when loading content. This is especially true when loading data into the application sandbox, where the generated SWF content can access the full set of AIR APIs.

There are legitimate uses for using the loadBytes() method without generating executable SWF code. You can use the loadBytes() method to generate an image data to control the timing of image display, for example. There are also legitimate uses that _do_ rely on executing code, such as dynamic creation of SWF content for audio playback. In AIR, by default the loadBytes() method does _not_ let you load SWF content; it only allows you to load image content. In AIR, the loaderContext property of the loadBytes() method has an allowLoadBytesCodeExecution property, which you can set to true to explicitly allow the application to use loadBytes() to load executable SWF content. The following code shows how to use this feature:

var loader:Loader = new Loader();

var loaderContext:LoaderContext = new LoaderContext(); loaderContext.allowLoadBytesCodeExecution = true; loader.loadBytes(bytes, loaderContext);

If you call loadBytes() to load SWF content and the allowLoadBytesCodeExecution property of the LoaderContext object is set to false (the default), the Loader object throws a SecurityError exception.

**_Note:_ **_In a future release of Adobe AIR, this API may change. When that occurs, you may need to recompile content that uses the allowLoadBytesCodeExecution property of the LoaderContext class._

### Best security practices for developers {#best-security-practices-for-developers}

Adobe AIR 1.0 and later

Although AIR applications are built using web technologies, it is important for developers to note that they are not working within the browser security sandbox. This means that it is possible to build AIR applications that can do harm to the local system, either intentionally or unintentionally. AIR attempts to minimize this risk, but there are still ways where vulnerabilities can be introduced. This topic covers important potential insecurities.

**Risk from importing files into the application security sandbox**

Adobe AIR 1.0 and later

Files that exist in the application directory are assigned to the application sandbox and have the full privileges of the runtime. Applications that write to the local file system are advised to write to app-storage:/. This directory exists separately from the application files on the user&#039;s computer, hence the files are not assigned to the application sandbox and present a reduced security risk. Developers are advised to consider the following:

*   Include a file in an AIR file (in the installed application) only if it is necessary.
*   Include a scripting file in an AIR file (in the installed application) only if its behavior is fully understood and trusted.
*   Do not write to or modify content in the application directory. The runtime prevents applications from writing or modifying files and directories using the app:/ URL scheme by throwing a SecurityError exception.
*   Do not use data from a network source as parameters to methods of the AIR API that may lead to code execution. This includes use of the Loader.loadBytes() method and the JavaScript eval() function.

Risk from using an external source to determine paths

Adobe AIR 1.0 and later

An AIR application can be compromised when using external data or content. For this reason, take special care when using data from the network or file system. The onus of trust is ultimately on the developer and the network connections they make, but loading foreign data is inherently risky, and should not be used for input into sensitive operations. Developers are advised against the following:

*   Using data from a network source to determine a file name
*   Using data from a network source to construct a URL that the application uses to send private information

Risk from using, storing, or transmitting insecure credentials

Adobe AIR 1.0 and later

Storing user credentials on the user&#039;s local file system inherently introduces the risk that these credentials may be compromised. Developers are advised to consider the following:

*   If credentials must be stored locally, encrypt the credentials when writing to the local file system. The runtime provides an encrypted storage unique to each installed application, via the EncryptedLocalStore class. For details, see

    “Encrypted local storage” on page 710

    .
*   Do not transmit unencrypted user credentials to a network source unless that source is trusted and the transmission uses the HTTPS: or Transport Layer Security (TLS) protocols.
*   Never specify a default password in credential creation — let users create their own. Users who leave the default unchanged expose their credentials to an attacker who already knows the default password.

Risk from a downgrade attack

Adobe AIR 1.0 and later

During application install, the runtime checks to ensure that a version of the application is not currently installed. If an application is already installed, the runtime compares the version string against the version that is being installed. If this string is different, the user can choose to upgrade their installation. The runtime does not guarantee that the newly installed version is newer than the older version, only that it is different. An attacker can distribute an older version to the user to circumvent a security weakness. For this reason, the developer is advised to make version checks when the application is run. It is a good idea to have applications check the network for required updates. That way, even if an attacker gets the user to run an old version, that old version will recognize that it needs to be updated. Also, using a clear versioning scheme for your application makes it more difficult to trick users into installing a downgraded version.

### Code signing {#code-signing}

Adobe AIR 1.0 and later

All AIR installer files are required to be code signed. Code signing is a cryptographic process of confirming that the specified origin of software is accurate. AIR applications can be signed using either by a certificate issued by an external certificate authority (CA) or by a self-signed certificate you create yourself. A commercial certificate from a well- known CA is strongly recommended and provides assurance to your users that they are installing your application, not a forgery. However, self-signed certificates can be created using adt from the SDK or using either Flash, Flash Builder, or another application that uses adt for certificate generation. Self-signed certificates do not provide any assurance that the application being installed is genuine and should only be used for testing an application prior to public release.

### Security on Android devices {#security-on-android-devices}

Adobe AIR 2.5 and later

On Android, as on all computing devices, AIR conforms to the native security model. At the same time, AIR maintains its own security rules, which are intended to make it easy for developers to write secure, Internet-connected applications.

Since AIR applications on Android use the Android package format, installation falls under the Android security model. The AIR application installer is not used.

The Android security model has three primary aspects:

*   Permissions
*   Application signatures
*   Application user IDs

Android permissions

Many features of Android are protected by the operating system permission mechanism. In order to use a protected feature, the AIR application descriptor must declare that the application requires the necessary permission. When a user attempts to install the app, the Android operating system displays all requested permissions to the user before the installation proceeds.

Most AIR applications will need to specify Android permissions in the application descriptor. By default, no permissions are included. The following permissions are required for protected Android features exposed through the AIR runtime:

**ACCESS_COARSE_LOCATION** Allows the application to access WIFI and cellular network location data through the Geolocation class.

**ACCESS_FINE_LOCATION** Allows the application to access GPS data through the Geolocation class.

**ACCESS_NETWORK_STATE and ACCESS_WIFI_STATE** Allows the application to access network information the NetworkInfo class.

**CAMERA** Allows the application to access the camera.

**INTERNET** Allows the application to make network requests. Also allows remote debugging. **READ_PHONE_STATE** Allows the AIR runtime to mute audio when an incoming call occurs. **RECORD_AUDIO** Allows the application to access the microphone.

**WAKE_LOCK and DISABLE_KEYGUARD** Allows the application to prevent the device from going to sleep using the SystemIdleMode class settings.

**WRITE_EXTERNAL_STORAGE** Allows the application to write to the external memory card on the device.

Application signatures

All application packages created for the Android platform must be signed. Since AIR applications on Android are packaged in the native Android APK format, they are signed in accordance to Android conventions rather than AIR conventions. While Android and AIR use code signing in a similar fashion, there are significant differences:

*   On Android, the signature verifies that the private key is in possession of the developer, but is not used to verify the identity of the developer.
*   For apps submitted to the Android market, the certificate must be valid for at least 25 years.
*   Android does not support migrating the package signature to another certificate. If an update is signed by a different certificate, then users must uninstall the original app before they can install the updated app.
*   Two apps signed with the same certificate can specify a shared ID that permits them to access each others cache and data files. (Such sharing is not facilitated by AIR. )

Application user IDs

Android uses a Linux kernel. Every installed app is assigned a Linux-type user ID that determines its permissions for such operations as file access. Files in the application, application storage, and temporary directories are protected from access by file system permissions. Files written to external storage (in other words, the SD card) can be read, modified, and deleted by other applications, or by the user, when the SD card is mounted as a mass storage device on a computer.

Cookies received with internet requests are not shared between AIR applications.

Background image privacy

When a user switches an application to the background, some Android versions capture a screenshot that it uses a thumbnail in the recent applications list. This screenshot is stored in device memory and can be accessed by an attacker in physical control of the device.

If your application displays sensitive information, you should guard against such information being captured by the background screenshot. The deactivate event dispatched by the NativeApplication object signals that an application is about to switch to the background. Use this event to clear or hide any sensitive information.

**More Help topics**

[Android: Security and Permissions](http://developer.android.com/guide/topics/security/security.html)

**Encrypted data on Android**

AIR applications on Android can use the encryption options available in the built-in SQL database to save encrypted data. For optimum security, base the encryption key on a user-entered password that is entered whenever the application is run. A locally stored encryption key or password is difficult or impossible to “hide” from an attacker who has access to the application files. If the attacker can retrieve the key, then encrypting the data does not afford any additional protection beyond the user ID-based filesystem security provided by the Android system.

The EncryptedLocalStore class can be used to save data, but that data is not encrypted on Android devices. Instead, the Android security model relies on the application user ID to protect the data from other applications. Applications that use a shared user ID and which are signed with the same code signing certificate use the same encrypted local store.

**_Important:_ **_On a rooted phone, any application running with root privileges can access the files of any other application. Thus, data stored using the encrypted local store is not secure on a rooted device._

### Security on iOS devices {#security-on-ios-devices}

On iOS AIR conforms to the native security model. At the same time, AIR maintains its own security rules, which are intended to make it easy for developers to write secure, Internet-connected applications.

Since AIR applications on iOS use the iOS package format, installation falls under the iOS security model. The AIR application installer is not used. Furthermore, a separate AIR runtime is not used on iOS devices. Every AIR application contains all the code needed to function.

Application signatures

All application packages created for the iOS platform must be signed. Since AIR applications on iOS are packaged in the native iOS IPA format, they are signed in accordance with iOS requirements rather than AIR requirements. While iOS and AIR use code signing in a similar fashion, there are significant differences:

*   On iOS, the certificate used to sign an application must be issued by Apple; Certificates from other certificate authorities cannot be used.
*   On iOS, Apple-issued distribution certificates are typically valid for one year.

Background image privacy

When a user switches an application to the background on iOS, the operating system captures a screenshot that it uses to animate the transition. This screenshot is stored in device memory and can be accessed by an attacker in physical control of the device.

If your application displays sensitive information, you should guard against such information being captured by the background screenshot. The deactivate event dispatched by the NativeApplication object signals that an application is about to switch to the background. Use this event to clear or hide any sensitive information.