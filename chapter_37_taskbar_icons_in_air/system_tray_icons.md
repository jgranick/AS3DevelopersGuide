## System Tray icons {#system-tray-icons}

Adobe AIR 1.0 and later

AIR supports system tray icons when NativeApplication.supportsSystemTrayIcon is true, which is currently the case only on Windows and most Linux distributions. On Windows and Linux, system tray icons are displayed in the notification area of the taskbar. No icon is displayed by default. To show an icon, assign an array containing BitmapData objects to the icon bitmaps property. To change the icon image, assign an array containing the new images to bitmaps. To remove the icon, set bitmaps to null.

**System tray icon menus**

Adobe AIR 1.0 and later

You can add a menu to the system tray icon by creating a NativeMenu object and assigning it to the NativeApplication.nativeApplication.icon.menu property (no default menu is provided by the operating system). Access the system tray icon menu by right-clicking the icon.

### System tray icon tooltips {#system-tray-icon-tooltips}

Adobe AIR 1.0 and later

Add a tooltip to an icon by setting the tooltip property:

NativeApplication.nativeApplication.icon.tooltip = &quot;Application name&quot;;

### System tray icon events {#system-tray-icon-events}

Adobe AIR 1.0 and later

The SystemTrayIcon object referenced by the NativeApplication.nativeApplication.icon property dispatches a ScreenMouseEvent for click, mouseDown, mouseUp, rightClick, rightMouseDown, and rightMouseUp events. You can use these events, along with an icon menu, to allow users to interact with your application when it has no visible windows.

### Example: Creating an application with no windows {#example-creating-an-application-with-no-windows}

Adobe AIR 1.0 and later

The following example creates an AIR application which has a system tray icon, but no visible windows. (The visible property of the application must not be set to true in the application descriptor, or the window will be visible when the application starts up.)

package

{

import flash.display.Loader; import flash.display.NativeMenu;

import flash.display.NativeMenuItem; import flash.display.NativeWindow; import flash.display.Sprite;

import flash.desktop.DockIcon; import flash.desktop.SystemTrayIcon; import flash.events.Event;

import flash.net.URLRequest;

import flash.desktop.NativeApplication;

public class SysTrayApp extends Sprite

{

public function SysTrayApp():void{ NativeApplication.nativeApplication.autoExit = false; var icon:Loader = new Loader();

var iconMenu:NativeMenu = new NativeMenu();

var exitCommand:NativeMenuItem = iconMenu.addItem(new NativeMenuItem(&quot;Exit&quot;)); exitCommand.addEventListener(Event.SELECT, function(event:Event):void {

});

NativeApplication.nativeApplication.icon.bitmaps = []; NativeApplication.nativeApplication.exit();

if (NativeApplication.supportsSystemTrayIcon) { NativeApplication.nativeApplication.autoExit = false; icon.contentLoaderInfo.addEventListener(Event.COMPLETE, iconLoadComplete); icon.load(new URLRequest(&quot;icons/AIRApp_16.png&quot;));

var systray:SystemTrayIcon = NativeApplication.nativeApplication.icon as SystemTrayIcon;

systray.tooltip = &quot;AIR application&quot;; systray.menu = iconMenu;

}

if (NativeApplication.supportsDockIcon){ icon.contentLoaderInfo.addEventListener(Event.COMPLETE,iconLoadComplete); icon.load(new URLRequest(&quot;icons/AIRApp_128.png&quot;));

var dock:DockIcon = NativeApplication.nativeApplication.icon as DockIcon; dock.menu = iconMenu;

}

}

private function iconLoadComplete(event:Event):void

{

NativeApplication.nativeApplication.icon.bitmaps = [event.target.content.bitmapData];

}

}

}

**_Note:_ **_When using the Flex WindowedApplication component, you must set the visible attribute of the WindowedApplication tag to false. This attribute supercedes the setting in the application descriptor._

**_Note:_ **_The example assumes that there are image files named AIRApp_16.png and AIRApp_128.png in an icons_

_subdirectory of the application. (Sample icon files, which you can copy to your project folder, are included in the AIR SDK.)_