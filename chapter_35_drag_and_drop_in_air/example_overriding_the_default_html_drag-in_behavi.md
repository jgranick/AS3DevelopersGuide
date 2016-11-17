## Example: Overriding the default HTML drag-in behavior {#example-overriding-the-default-html-drag-in-behavior}

Adobe AIR 1.0 and later

This example implements a drop target that displays a table showing each data format available in the dropped item.

The default behavior is used to allow text, links, and images to be dragged within the application. The example overrides the default drag-in behavior for the div element that serves as the drop target. The key step to enabling non- editable content to accept a drag-in gesture is to call the preventDefault() method of the event object dispatched for both the dragenter and dragover events. In response to a drop event, the handler converts the transferred data into an HTML row element and inserts the row into a table for display.

&lt;html&gt;

&lt;head&gt;

&lt;title&gt;Drag-and-drop&lt;/title&gt;

&lt;script language=&quot;javascript&quot; type=&quot;text/javascript&quot; src=&quot;AIRAliases.js&quot;&gt;&lt;/script&gt;

&lt;script language=&quot;javascript&quot;&gt; function init(){

var target = document.getElementById(&#039;target&#039;); target.addEventListener(&quot;dragenter&quot;, dragEnterOverHandler); target.addEventListener(&quot;dragover&quot;, dragEnterOverHandler); target.addEventListener(&quot;drop&quot;, dropHandler);

var source = document.getElementById(&#039;source&#039;); source.addEventListener(&quot;dragstart&quot;, dragStartHandler); source.addEventListener(&quot;dragend&quot;, dragEndHandler);

emptyRow = document.getElementById(&quot;emptyTargetRow&quot;);

}

function dragStartHandler(event){ event.dataTransfer.effectAllowed = &quot;copy&quot;;

}

function dragEndHandler(event){

air.trace(event.type + &quot;: &quot; + event.dataTransfer.dropEffect);

}

function dragEnterOverHandler(event){ event.preventDefault();

}

var emptyRow;

function dropHandler(event){ for(var prop in event){

air.trace(prop + &quot; = &quot; + event[prop]);

}

var row = document.createElement(&#039;tr&#039;);

row.innerHTML = &quot;&lt;td&gt;&quot; + event.dataTransfer.getData(&quot;text/plain&quot;) + &quot;&lt;/td&gt;&quot; + &quot;&lt;td&gt;&quot; + event.dataTransfer.getData(&quot;text/html&quot;) + &quot;&lt;/td&gt;&quot; +

&quot;&lt;td&gt;&quot; + event.dataTransfer.getData(&quot;text/uri-list&quot;) + &quot;&lt;/td&gt;&quot; +

&quot;&lt;td&gt;&quot; + event.dataTransfer.getData(&quot;application/x-vnd.adobe.air.file-list&quot;) + &quot;&lt;/td&gt;&quot;;

1){

var imageCell = document.createElement(&#039;td&#039;); if((event.dataTransfer.types.toString()).search(&quot;image/x-vnd.adobe.air.bitmap&quot;) &gt; -

imageCell.appendChild(event.dataTransfer.getData(&quot;image/x-

vnd.adobe.air.bitmap&quot;));

}

row.appendChild(imageCell);

var parent = emptyRow.parentNode; parent.insertBefore(row, emptyRow);

}

&lt;/script&gt;

&lt;/head&gt;

&lt;body onLoad=&quot;init()&quot; style=&quot;padding:5px&quot;&gt;

&lt;div&gt;

&lt;h1&gt;Source&lt;/h1&gt;

&lt;p&gt;Items to drag:&lt;/p&gt;

&lt;ul id=&quot;source&quot;&gt;

&lt;li&gt;Plain text.&lt;/li&gt;

&lt;li&gt;HTML &lt;b&gt;formatted&lt;/b&gt; text.&lt;/li&gt;

&lt;li&gt;A &lt;a [href=&quot;http://www.adobe.com&quot;&gt;URL.&lt;/a&gt;&lt;/li&gt;](http://www.adobe.com/)

&lt;li&gt;&lt;img src=&quot;icons/AIRApp_16.png&quot; alt=&quot;An image&quot;/&gt;&lt;/li&gt;

&lt;li style=&quot;-webkit-user-drag:none;&quot;&gt; Uses &quot;-webkit-user-drag:none&quot; style.

&lt;/li&gt;

&lt;li style=&quot;-webkit-user-select:none;&quot;&gt; Uses &quot;-webkit-user-select:none&quot; style.

&lt;/li&gt;

&lt;/ul&gt;

&lt;/div&gt;

&lt;div id=&quot;target&quot; style=&quot;border-style:dashed;&quot;&gt;

&lt;h1 &gt;Target&lt;/h1&gt;

&lt;p&gt;Drag items from the source list (or elsewhere).&lt;/p&gt;

&lt;table id=&quot;displayTable&quot; border=&quot;1&quot;&gt;

&lt;tr&gt;&lt;th&gt;Plain text&lt;/th&gt;&lt;th&gt;Html text&lt;/th&gt;&lt;th&gt;URL&lt;/th&gt;&lt;th&gt;File list&lt;/th&gt;&lt;th&gt;Bitmap Data&lt;/th&gt;&lt;/tr&gt;

&lt;tr id=&quot;emptyTargetRow&quot;&gt;&lt;td&gt;&amp;nbsp;&lt;/td&gt;&lt;td&gt;&amp;nbsp;&lt;/td&gt;&lt;td&gt;&amp;nbsp;&lt;/td&gt;&lt;td&gt;&amp;nbsp;&lt;/td&gt;&lt;td&gt;&amp;nbsp;&lt;/ td&gt;&lt;/tr&gt;

&lt;/table&gt;

&lt;/div&gt;

&lt;/div&gt;

&lt;/body&gt;

&lt;/html&gt;