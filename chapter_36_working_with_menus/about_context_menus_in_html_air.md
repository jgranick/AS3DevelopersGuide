## About context menus in HTML (AIR) {#about-context-menus-in-html-air}

Adobe AIR 1.0 and later

In HTML content displayed using the HTMLLoader object, the contextmenu event can be used to display a context menu. By default, a context menu is displayed automatically when the user invokes the context menu event on selected text (by right-clicking or command-clicking the text). To prevent the default menu from opening, listen for the contextmenu event and call the event object’s preventDefault() method:

function showContextMenu(event){ event.preventDefault();

}

You can then display a custom context menu using DHTML techniques or by displaying an AIR native context menu. The following example displays a native context menu by calling the menu display() method in response to the HTML contextmenu event:

&lt;html&gt;

&lt;head&gt;

&lt;script src=&quot;AIRAliases.js&quot; language=&quot;JavaScript&quot; type=&quot;text/javascript&quot;&gt;&lt;/script&gt;

&lt;script language=&quot;javascript&quot; type=&quot;text/javascript&quot;&gt;

function showContextMenu(event){ event.preventDefault();

contextMenu.display(window.nativeWindow.stage, event.clientX, event.clientY);

}

function createContextMenu(){

var menu = new air.NativeMenu();

var command = menu.addItem(new air.NativeMenuItem(&quot;Custom command&quot;)); command.addEventListener(air.Event.SELECT, onCommand);

return menu;

}

function onCommand(){

air.trace(&quot;Context command invoked.&quot;);

}

var contextMenu = createContextMenu();

&lt;/script&gt;

&lt;/head&gt;

&lt;body&gt;

&lt;p oncontextmenu=&quot;showContextMenu(event)&quot; style=&quot;-khtml-user-select:auto;&quot;&gt;Custom context menu.&lt;/p&gt;

&lt;/body&gt;

&lt;/html&gt;