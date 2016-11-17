## Window taskbar icons and buttons {#window-taskbar-icons-and-buttons}

Adobe AIR 1.0 and later

Iconified representations of windows are typically displayed in the window area of a taskbar or dock to allow users to easily access background or minimized windows. The Mac OS X dock displays an icon for your application as well as an icon for each minimized window. The Microsoft Windows and Linux taskbars display a button containing the progam icon and title for each normal-type window in your application.

**Highlighting the taskbar window button**

Adobe AIR 1.0 and later

When a window is in the background, you can notify the user that an event of interest related to the window has occurred. On Mac OS X, you can notify the user by bouncing the application dock icon (as described in

“Bouncing the

dock” on page 649

). On Windows and Linux, you can highlight the window taskbar button by calling the notifyUser() method of the NativeWindow instance. The type parameter passed to the method determines the urgency of the notification:

• NotificationType.CRITICAL: the window icon flashes until the user brings the window to the foreground.

• NotificationType.INFORMATIONAL: the window icon highlights by changing color.

**_Note:_ **_On Linux, only the informational type of notification is supported. Passing either type value to the_

_notifyUser() function will create the same effect._

The following statement highlights the taskbar button of a window:

stage.nativeWindow.notifyUser(NotificationType.CRITICAL);

Calling the NativeWindow.notifyUser() method on an operating system that does not support window-level notification has no effect. Use the NativeWindow.supportsNotification property to determine if window notification is supported.

### Creating windows without taskbar buttons or icons {#creating-windows-without-taskbar-buttons-or-icons}

Adobe AIR 1.0 and later

On the Windows operating system, windows created with the types _utility_ or _lightweight_ do not appear on the taskbar. Invisible windows do not appear on the taskbar, either.

Because the initial window is necessarily of type, _normal_, in order to create an application without any windows appearing in the taskbar, you must either close the initial window or leave it invisible. To close all windows in your application without terminating the application, set the autoExit property of the NativeApplication object to false before closing the last window. To simply prevent the initial window from ever becoming visible, add

&lt;visible&gt;false&lt;/visible&gt; to the &lt;initalWindow&gt; element of the application descriptor file (and do not set the

visible property to true or call the activate() method of the window).

In new windows opened by the application, set the type property of the NativeWindowInitOption object passed to the window constructor to NativeWindowType.UTILITY or NativeWindowType.LIGHTWEIGHT.

On Mac OS X, windows that are minimized are displayed on the dock taskbar. You can prevent the minimized icon from being displayed by hiding the window instead of minimizing it. The following example listens for a nativeWindowDisplayState change event and cancels it if the window is being minimized. Instead the handler sets the window visible property to false:

private function preventMinimize(event:NativeWindowDisplayStateEvent):void{ if(event.afterDisplayState == NativeWindowDisplayState.MINIMIZED){

event.preventDefault(); event.target.visible = false;

}

}

If a window is minimized on the Mac OS X dock when you set the visible property to false, the dock icon is not removed. A user can still click the icon to make the window reappear.