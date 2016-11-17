## Detecting permissions for camera access {#detecting-permissions-for-camera-access}

Flash Player 9 and later, Adobe AIR 1.0 and later

In the AIR application sandbox, the application can access any camera without the user&#039;s permission. On Android, however, the application must specify the Android CAMERA permission in the application descriptor.

Before Flash Player can display a camera’s output, the user must explicitly allow Flash Player to access the camera. When the attachCamera() method gets called Flash Player displays the Flash Player Settings dialog box which prompts the user to either allow or deny Flash Player access to the camera and microphone. If the user clicks the Allow button, Flash Player displays the camera’s output in the Video instance on the Stage. If the user clicks the Deny button, Flash Player is unable to connect to the camera and the Video object does not display anything.

If you want to detect whether the user allowed Flash Player access to the camera, you can listen for the camera’s status

event (StatusEvent.STATUS), as seen in the following code:

var cam:Camera = Camera.getCamera(); if (cam != null)

{

cam.addEventListener(StatusEvent.STATUS, statusHandler); var vid:Video = new Video();

vid.attachCamera(cam); addChild(vid);

}

function statusHandler(event:StatusEvent):void

{

// This event gets dispatched when the user clicks the &quot;Allow&quot; or &quot;Deny&quot;

// button in the Flash Player Settings dialog box. trace(event.code); // &quot;Camera.Muted&quot; or &quot;Camera.Unmuted&quot;

}

The statusHandler() function gets called as soon as the user clicks either Allow or Deny. You can detect which button the user clicked, using one of two methods:

• The event parameter of the statusHandler() function contains a code property which contains the string “Camera.Muted” or “Camera.Unmuted”. If the value is “Camera.Muted” the user clicked the Deny button and Flash Player is unable to access the camera. You can see an example of this in the following snippet:

function statusHandler(event:StatusEvent):void

{

switch (event.code)

{

case &quot;Camera.Muted&quot;: trace(&quot;User clicked Deny.&quot;); break;

case &quot;Camera.Unmuted&quot;: trace(&quot;User clicked Accept.&quot;); break;

}

}

• The Camera class contains a read-only property named muted which specifies whether the user has denied access to the camera (true) or allowed access (false) in the Flash Player Privacy panel. You can see an example of this in the following snippet:

function statusHandler(event:StatusEvent):void

{

if (cam.muted)

{

trace(&quot;User clicked Deny.&quot;);

}

else

{

trace(&quot;User clicked Accept.&quot;);

}

}

By checking for the status event to be dispatched, you can write code that handles the user accepting or denying access to the camera and clean up appropriately. For example, if the user clicks the Deny button, you could display a message to the user stating that they need to click Allow if they want to participate in a video chat, or you could instead make sure the Video object on the display list is deleted to free up system resources.

In AIR, a Camera object does not dispatch status events since permission to use the camera is not dynamic.