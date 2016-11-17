## Dock icons {#dock-icons}

Adobe AIR 1.0 and later

AIR supports dock icons when NativeApplication.supportsDockIcon is true. The NativeApplication.nativeApplication.icon property represents the application icon on the dock (not a window dock icon).

**_Note:_ **_AIR does not support changing window icons on the dock under Mac OS X. Also, changes to the application dock icon only apply while an application is running — the icon reverts to its normal appearance when the application terminates._

**Dock icon menus**

Adobe AIR 1.0 and later

You can add commands to the standard dock menu by creating a NativeMenu object containing the commands and assigning it to the NativeApplication.nativeApplication.icon.menu property. The items in the menu are displayed above the standard dock icon menu items.

### Bouncing the dock {#bouncing-the-dock}

Adobe AIR 1.0 and later

You can bounce the dock icon by calling the NativeApplication.nativeApplication.icon.bounce() method. If you set the bounce() priority parameter to informational, then the icon bounces once. If you set it to critical, then the icon bounces until the user activates the application. Constants for the priority parameter are defined in the NotificationType class.

**_Note:_ **_The icon does not bounce if the application is already active._

### Dock icon events {#dock-icon-events}

Adobe AIR 1.0 and later

When the dock icon is clicked, the NativeApplication object dispatches an invoke event. If the application is not running, the system launches it. Otherwise, the invoke event is delivered to the running application instance.