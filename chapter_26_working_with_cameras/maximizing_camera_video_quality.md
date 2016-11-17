## Maximizing camera video quality {#maximizing-camera-video-quality}

Flash Player 9 and later, Adobe AIR 1.0 and later

By default, new instances of the Video class are 320 pixels wide by 240 pixels high. In order to maximize video quality you should always ensure that your video object matches the same dimensions as the video being returned by the camera object. You can get the camera object’s width and height by using the Camera class’s width and height properties, you can then set the video object’s width and height properties to match the camera objects dimensions, or you can pass the camera’s width and height to the Video class’s constructor method, as seen in the following snippet:

var cam:Camera = Camera.getCamera(); if (cam != null)

{

var vid:Video = new Video(cam.width, cam.height); vid.attachCamera(cam);

addChild(vid);

}

Since the getCamera() method returns a reference to a camera object (or null if no cameras are available) you can access the camera’s methods and properties even if the user denies access to their camera. This allows you to set the size of the video instance using the camera’s native height and width.

var vid:Video;

var cam:Camera = Camera.getCamera();

if (cam == null)

{

trace(&quot;Unable to locate available cameras.&quot;);

}

else

{

trace(&quot;Found camera: &quot; + cam.name); cam.addEventListener(StatusEvent.STATUS, statusHandler); vid = new Video();

vid.attachCamera(cam);

}

function statusHandler(event:StatusEvent):void

{

if (cam.muted)

{

trace(&quot;Unable to connect to active camera.&quot;);

}

else

{

// Resize Video object to match camera settings and

// add the video to the display list. vid.width = cam.width;

vid.height = cam.height; addChild(vid);

}

// Remove the status event listener. cam.removeEventListener(StatusEvent.STATUS, statusHandler);

}

For information about full-screen mode, see the full-screen mode section under

“Setting Stage properties” on

page 164

.