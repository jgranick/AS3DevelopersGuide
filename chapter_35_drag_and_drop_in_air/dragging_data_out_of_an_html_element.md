## Dragging data out of an HTML element {#dragging-data-out-of-an-html-element}

Adobe AIR 1.0 and later

The default behavior allows most content in an HTML page to be copied by dragging. You can control the content allowed to be dragged using CSS properties -webkit-user-select and -webkit-user-drag.

Override the default drag-out behavior in the handler for the dragstart event. Call the setData() method of the

dataTransfer property of the event object to put your own data into the drag gesture.

To indicate which drag effects a source object supports when you are not relying on the default behavior, set the dataTransfer.effectAllowed property of the event object dispatched for the dragstart event. You can choose any combination of effects. For example, if a source element supports both _copy_ and _link_ effects, set the property to &quot;copyLink&quot;.

**Setting the dragged data**

Flash Player 9 and later, Adobe AIR 1.0 and later

Add the data for the drag gesture in the handler for the dragstart event with the dataTransfer property. Use the

dataTransfer.setData() method to put data onto the clipboard, passing in the MIME type and the data to transfer.

For example, if you had an image element in your application, with the id _imageOfGeorge_, you could use the following dragstart event handler. This example adds representations of a picture of George in several data formats, which increases the likelihood that other applications can use the dragged data.

function dragStartHandler(event){ event.dataTransfer.effectAllowed = &quot;copy&quot;;

var dragImage = document.getElementById(&quot;imageOfGeorge&quot;); var dragFile = new air.File(dragImage.src);

event.dataTransfer.setData(&quot;text/plain&quot;,&quot;A picture of George&quot;); event.dataTransfer.setData(&quot;image/x-vnd.adobe.air.bitmap&quot;, dragImage); event.dataTransfer.setData(&quot;application/x-vnd.adobe.air.file-list&quot;,

new Array(dragFile));

}

**_Note:_ **_When you call the setData() method of dataTransfer object, no data is added by the default drag-and-drop behavior._