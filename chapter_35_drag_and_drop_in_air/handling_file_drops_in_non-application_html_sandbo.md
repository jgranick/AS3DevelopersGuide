## Handling file drops in non-application HTML sandboxes {#handling-file-drops-in-non-application-html-sandboxes}

Adobe AIR 1.0 and later

Non-application content cannot access the File objects that result when files are dragged into an AIR application. Nor is it possible to pass one of these File objects to application content through a sandbox bridge. (The object properties must be accessed during serialization.) However, you can still drop files in your application by listening for the AIR nativeDragDrop events on the HTMLLoader object.

Normally, if a user drops a file into a frame that hosts non-application content, the drop event does not propagate from the child to the parent. However, since the events dispatched by the HTMLLoader (which is the container for all HTML content in an AIR application) are not part of the HTML event flow, you can still receive the drop event in application content.

To receive the event for a file drop, the parent document adds an event listener to the HTMLLoader object using the reference provided by window.htmlLoader:

window.htmlLoader.addEventListener(&quot;nativeDragDrop&quot;,function(event){

var filelist = event.clipboard.getData(air.ClipboardFormats.FILE_LIST_FORMAT); air.trace(filelist[0].url);

});

The following example uses a parent document that loads a child page into a remote sandbox [(http://localhost/).](http://localhost/)) The parent listens for the nativeDragDrop event on the HTMLLoader object and traces out the file url.

&lt;html&gt;

&lt;head&gt;

&lt;title&gt;Drag-and-drop in a remote sandbox&lt;/title&gt;

&lt;script language=&quot;javascript&quot; type=&quot;text/javascript&quot; src=&quot;AIRAliases.js&quot;&gt;&lt;/script&gt;

&lt;script language=&quot;javascript&quot;&gt; window.htmlLoader.addEventListener(&quot;nativeDragDrop&quot;,function(event){

var filelist = event.clipboard.getData(air.ClipboardFormats.FILE_LIST_FORMAT); air.trace(filelist[0].url);

});

&lt;/script&gt;

&lt;/head&gt;

&lt;body&gt;

&lt;iframe src=&quot;child.html&quot;

[sandboxRoot=&quot;http://localhost/&quot;](http://localhost/) documentRoot=&quot;app:/&quot;

frameBorder=&quot;0&quot; width=&quot;100%&quot; height=&quot;100%&quot;&gt;

&lt;/iframe&gt;

&lt;/body&gt;

&lt;/html&gt;

The child document must present a valid drop target by calling the Event object preventDefault() method in the HTML dragenter and dragover event handlers. Otherwise, the drop event can never occur.

&lt;html&gt;

&lt;head&gt;

&lt;title&gt;Drag and drop target&lt;/title&gt;

&lt;script language=&quot;javascript&quot; type=&quot;text/javascript&quot;&gt; function preventDefault(event){

event.preventDefault();

}

&lt;/script&gt;

&lt;/head&gt;

&lt;body ondragenter=&quot;preventDefault(event)&quot; ondragover=&quot;preventDefault(event)&quot;&gt;

&lt;div&gt;

&lt;h1&gt;Drop Files Here&lt;/h1&gt;

&lt;/div&gt;

&lt;/body&gt;

&lt;/html&gt;