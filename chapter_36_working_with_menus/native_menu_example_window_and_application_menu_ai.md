## Native menu example: Window and application menu (AIR) {#native-menu-example-window-and-application-menu-air}

Adobe AIR 1.0 and later

The following example creates the menu shown in

“Native menu structure (AIR)” on page 635

.

The menu is designed to work both on Windows, for which only window menus are supported, and on Mac OS X, for which only application menus are supported. To make the distinction, the MenuExample class constructor checks the static supportsMenu properties of the NativeWindow and NativeApplication classes. If NativeWindow.supportsMenu is true, then the constructor creates a NativeMenu object for the window and then creates and adds the File and Edit submenus. If NativeApplication.supportsMenu is true, then the constructor creates and adds the File and Edit menus to the existing menu provided by the Mac OS X operating system.

The example also illustrates menu event handling. The select event is handled at the item level and also at the menu level. Each menu in the chain from the menu containing the selected item to the root menu responds to the select event. The displaying event is used with the “Open Recent” menu. Just before the menu is opened, the items in the menu are refreshed from the recent Documents array (which doesn’t actually change in this example). Although not shown in this example, you can also listen for displaying events on individual items.

package {

import flash.display.NativeMenu; import flash.display.NativeMenuItem; import flash.display.NativeWindow; import flash.display.Sprite;

import flash.events.Event; import flash.filesystem.File;

import flash.desktop.NativeApplication;

public class MenuExample extends Sprite

{

private var recentDocuments:Array =

new Array(new File(&quot;app-storage:/GreatGatsby.pdf&quot;), new File(&quot;app-storage:/WarAndPeace.pdf&quot;), new File(&quot;app-storage:/Iliad.pdf&quot;));

public function MenuExample()

{

var fileMenu:NativeMenuItem; var editMenu:NativeMenuItem;

if (NativeWindow.supportsMenu){ stage.nativeWindow.menu = new NativeMenu();

stage.nativeWindow.menu.addEventListener(Event.SELECT, selectCommandMenu); fileMenu = stage.nativeWindow.menu.addItem(new NativeMenuItem(&quot;File&quot;)); fileMenu.submenu = createFileMenu();

editMenu = stage.nativeWindow.menu.addItem(new NativeMenuItem(&quot;Edit&quot;)); editMenu.submenu = createEditMenu();

}

if (NativeApplication.supportsMenu){ NativeApplication.nativeApplication.menu.addEventListener(Event.SELECT,

selectCommandMenu);

fileMenu = NativeApplication.nativeApplication.menu.addItem(new NativeMenuItem(&quot;File&quot;));

fileMenu.submenu = createFileMenu();

editMenu = NativeApplication.nativeApplication.menu.addItem(new NativeMenuItem(&quot;Edit&quot;));

editMenu.submenu = createEditMenu();

}

}

public function createFileMenu():NativeMenu {

var fileMenu:NativeMenu = new NativeMenu(); fileMenu.addEventListener(Event.SELECT, selectCommandMenu);

var newCommand:NativeMenuItem = fileMenu.addItem(new NativeMenuItem(&quot;New&quot;)); newCommand.addEventListener(Event.SELECT, selectCommand);

var saveCommand:NativeMenuItem = fileMenu.addItem(new NativeMenuItem(&quot;Save&quot;)); saveCommand.addEventListener(Event.SELECT, selectCommand);

var openRecentMenu:NativeMenuItem =

fileMenu.addItem(new NativeMenuItem(&quot;Open Recent&quot;)); openRecentMenu.submenu = new NativeMenu(); openRecentMenu.submenu.addEventListener(Event.DISPLAYING,

updateRecentDocumentMenu); openRecentMenu.submenu.addEventListener(Event.SELECT, selectCommandMenu);

return fileMenu;

}

public function createEditMenu():NativeMenu { var editMenu:NativeMenu = new NativeMenu();

editMenu.addEventListener(Event.SELECT, selectCommandMenu);

var copyCommand:NativeMenuItem = editMenu.addItem(new NativeMenuItem(&quot;Copy&quot;)); copyCommand.addEventListener(Event.SELECT, selectCommand); copyCommand.keyEquivalent = &quot;c&quot;;

var pasteCommand:NativeMenuItem = editMenu.addItem(new NativeMenuItem(&quot;Paste&quot;));

pasteCommand.addEventListener(Event.SELECT, selectCommand); pasteCommand.keyEquivalent = &quot;v&quot;;

editMenu.addItem(new NativeMenuItem(&quot;&quot;, true)); var preferencesCommand:NativeMenuItem =

editMenu.addItem(new NativeMenuItem(&quot;Preferences&quot;)); preferencesCommand.addEventListener(Event.SELECT, selectCommand);

return editMenu;

}

private function updateRecentDocumentMenu(event:Event):void { trace(&quot;Updating recent document menu.&quot;);

var docMenu:NativeMenu = NativeMenu(event.target);

for each (var item:NativeMenuItem in docMenu.items) { docMenu.removeItem(item);

}

for each (var file:File in recentDocuments) { var menuItem:NativeMenuItem =

docMenu.addItem(new NativeMenuItem(file.name)); menuItem.data = file;

menuItem.addEventListener(Event.SELECT, selectRecentDocument);

}

}

private function selectRecentDocument(event:Event):void { trace(&quot;Selected recent document: &quot; + event.target.data.name);

}

private function selectCommand(event:Event):void {

trace(&quot;Selected command: &quot; + event.target.label);

}

private function selectCommandMenu(event:Event):void { if (event.currentTarget.parent != null) {

var menuItem:NativeMenuItem = findItemForMenu(NativeMenu(event.currentTarget));

if (menuItem != null) { trace(&quot;Select event for \&quot;&quot; +

event.target.label +

&quot;\&quot; command handled by menu: &quot; + menuItem.label);

}

} else {

trace(&quot;Select event for \&quot;&quot; + event.target.label +

&quot;\&quot; command handled by root menu.&quot;);

}

}

private function findItemForMenu(menu:NativeMenu):NativeMenuItem { for each (var item:NativeMenuItem in menu.parent.items) {

if (item != null) {

if (item.submenu == menu) { return item;

}

}

}

return null;

}

}

}