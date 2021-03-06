## Updating Flash Player to support Adobe Access {#updating-flash-player-to-support-adobe-access}

Flash Player 10.1 and later

**_Important_**: Flash Player 11.5 and above integrates the Adobe Access module, so the update step (calling SystemUpdater.update(SystemUpdaterType.DRM)) is unnecessary. This includes the following browsers and platforms:

• Flash Player 11.5 ActiveX control, for all platforms except Internet Explorer on Windows 8

• Flash Player 11.5 plugin, for all browsers

• Adobe AIR (desktop and mobile)

This means that the update step is _still required_ in the following cases:

• Internet Explorer on Windows 8

• Flash Player 11.4 and below, except on Google Chrome 22 and above (all platforms) or 21 and above (Windows)

**_Note:_ **_You can still safely call SystemUpdater.update(SystemUpdaterType.DRM)on a system with Flash Player 11.5 or higher, but nothing is downloaded._

To support Adobe Access, Flash Player requires the Adobe Access module. When Flash Player tries to play protected content, the runtime indicates if the module or a new version of Flash Player must be downloaded. In this way, Flash Player allows SWF developers the option of not updating if desired.

In most cases, to play protected content, SWF developers update to the required Adobe Access module or player. To update, you can use the SystemUpdater API to get the latest version of the Adobe Access module or of Flash Player.

The SystemUpdater API permits only one update at a time. The error code 2202 indicates that an update is already occurring in the current runtime instance or another instance. For example, if an update is occurring in a Flash Player instance in Internet Explorer, an update cannot proceed in a Flash Player instance running in Firefox.

The SystemUpdater API is supported for desktop platforms only.

**_Note:_ **_For versions of Flash Player earlier than 10.1, use the update mechanism supported in earlier player versions (manual download and install from_ [_www.adobe.com_](http://www.adobe.com/) _or ExpressInstall). Also, the AIR installer handles necessary updates for Adobe Access and does not support the SystemUpdater API._

**Listening for an update event**

Flash Player 10.1 and later

When an update of the Adobe Access module is required, the NetStream object dispatches a NetStatusEvent with a code value of DRM.UpdateNeeded. This value indicates that the NetStream object cannot play back the protected stream with any of the currently installed Adobe Access modules. Listen for this event and call the following code:

SystemUpdater.update(flash.system.SystemUpdaterType.DRM)

This code updates the Adobe Access module installed in the player. User consent for this module update is not required.

If the Adobe Access module is not found, an error is thrown. See step 3 of the

“Detailed API workflow” on page 530

.

**_Note:_ **_If play() is called on an encrypted stream in players earlier than 10.1, a NetStatusEvent with code value of NetStream.Play.StreamNotFound is dispatched. For earlier players, use the update mechanism supported for those players (manual download and install from_ [_www.adobe.com_](http://www.adobe.com/) _or ExpressInstall)._

When an update of the player itself is required, the SystemUpdater object dispatches a StatusEvent with a code value of DRM.UpdateNeededButIncompatible is dispatched. For an update of the player, user consent is required. In your application, provide an interface for the user to agree to and initiate the update of the player. Listen for the StatusEvent event and call the following code:

SystemUpdater.update(flash.system.SystemUpdaterType.SYSTEM);

This code initiates the update of the player.

Additional events for the SystemUpdater class are documented in the [ActionScript 3.0 Reference for the Adobe Flash](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/system/SystemUpdater.html#eventSummary) [Platform](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/system/SystemUpdater.html#eventSummary).

After the player update completes, the user is redirected to the page where the update began. The Adobe Access module is downloaded, and the stream can begin playing.