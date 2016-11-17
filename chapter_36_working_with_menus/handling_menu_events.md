## Handling menu events {#handling-menu-events}

Flash Player 9 and later, Adobe AIR 1.0 and later

A menu dispatches events when the user selects the menu or when the user selects a menu item.

**Events summary for menu classes**

Flash Player 9 and later, Adobe AIR 1.0 and later

Add event listeners to menus or individual items to handle menu events.

| **Object** | **Events dispatched** |
| --- | --- |
| NativeMenu (AIR) | Event.PREPARING (Adobe AIR 2.6 and later) Event.DISPLAYING |
| NativeMenuItem (AIR) | Event.PREPARING (Adobe AIR 2.6 and later) Event.SELECT |
| ContextMenu | ContextMenuEvent.MENU_SELECT |
| ContextMenuItem | ContextMenuEvent.MENU_ITEM_SELECT Event.SELECT (AIR) |

### Select menu events {#select-menu-events}

Adobe AIR 1.0 and later

To handle a click on a menu item, add an event listener for the select event to the NativeMenuItem object:

var menuCommandX:NativeMenuItem = new NativeMenuItem(&quot;Command X&quot;); menuCommandX.addEventListener(Event.SELECT, doCommandX)

Because select events bubble up to the containing menus, you can also listen for select events on a parent menu. When listening at the menu level, you can use the event object target property to determine which menu command was selected. The following example traces the label of the selected command:

var colorMenuItem:NativeMenuItem = new NativeMenuItem(&quot;Choose a color&quot;); var colorMenu:NativeMenu = new NativeMenu();

colorMenuItem.submenu = colorMenu;

var red:NativeMenuItem = new NativeMenuItem(&quot;Red&quot;);

var green:NativeMenuItem = new NativeMenuItem(&quot;Green&quot;); var blue:NativeMenuItem = new NativeMenuItem(&quot;Blue&quot;); colorMenu.addItem(red);

colorMenu.addItem(green); colorMenu.addItem(blue);

if(NativeApplication.supportsMenu){ NativeApplication.nativeApplication.menu.addItem(colorMenuItem); NativeApplication.nativeApplication.menu.addEventListener(Event.SELECT, colorChoice);

} else if (NativeWindow.supportsMenu){

var windowMenu:NativeMenu = new NativeMenu(); this.stage.nativeWindow.menu = windowMenu; windowMenu.addItem(colorMenuItem); windowMenu.addEventListener(Event.SELECT, colorChoice);

}

function colorChoice(event:Event):void {

var menuItem:NativeMenuItem = event.target as NativeMenuItem; trace(menuItem.label + &quot; has been selected&quot;);

}

If you are using the ContextMenuItem class, you can listen for either the select event or the menuItemSelect event. The menuItemSelect event gives you additional information about the object owning the context menu, but does not bubble up to the containing menus.

### Displaying menu events {#displaying-menu-events}

Adobe AIR 1.0 and later

To handle the opening of a menu, you can add a listener for the displaying event, which is dispatched before a menu is displayed. You can use the displaying event to update the menu, for example by adding or removing items, or by updating the enabled or checked states of individual items. You can also listen for the menuSelect event from a ContextMenu object.

In AIR 2.6 and later, you can use the preparing event to update a menu in response to either displaying a menu or selecting an item with a keyboard shortcut.