# Chapter 37: Taskbar icons in AIR {#chapter-37-taskbar-icons-in-air}

Adobe AIR 1.0 and later

Many operating systems provide a taskbar, such as the Mac OS X dock, that can contain an icon to represent an application. Adobe® AIR® provides an interface for interacting with the application task bar icon through the NativeApplication.nativeApplication.icon property.

• [Using the system tray and dock icons](http://www.adobe.com/go/learn_air_qs_systray_en) (Flex)

• [Using the system tray and dock icons](http://www.adobe.com/go/learn_air_qs_systray_flash_en) (Flash)

**More Help topics**

[flash.desktop.NativeApplication](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/desktop/NativeApplication.html) [flash.desktop.DockIcon](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/desktop/DockIcon.html) [flash.desktop.SystemTrayIcon](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/desktop/SystemTrayIcon.html)

**About taskbar icons**

Adobe AIR 1.0 and later

AIR creates the NativeApplication.nativeApplication.icon object automatically. The object type is either DockIcon or SystemTrayIcon, depending on the operating system. You can determine which of these InteractiveIcon subclasses that AIR supports on the current operating system using the NativeApplication.supportsDockIcon and NativeApplication.supportsSystemTrayIcon properties. The InteractiveIcon base class provides the properties width, height, and bitmaps, which you can use to change the image used for the icon. However, accessing properties specific to DockIcon or SystemTrayIcon on the wrong operating system generates a runtime error.

To set or change the image used for an icon, create an array containing one or more images and assign it to the NativeApplication.nativeApplication.icon.bitmaps property. The size of taskbar icons can be different on different operating systems. To avoid image degradation due to scaling, you can add multiple sizes of images to the bitmaps array. If you provide more than one image, AIR selects the size closest to the current display size of the taskbar icon, scaling it only if necessary. The following example sets the image for a taskbar icon using two images:

NativeApplication.nativeApplication.icon.bitmaps = [bmp16x16.bitmapData, bmp128x128.bitmapData];

To change the icon image, assign an array containing the new image or images to the bitmaps property. You can animate the icon by changing the image in response to an enterFrame or timer event.

To remove the icon from the notification area on Windows and Linux, or to restore the default icon appearance on Mac OS X, set bitmaps to an empty array:

NativeApplication.nativeApplication.icon.bitmaps = [];