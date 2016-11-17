## Verifying that cameras are installed {#verifying-that-cameras-are-installed}

Flash Player 9 and later, Adobe AIR 1.0 and later

Before you attempt to use any methods or properties on a camera instance, you’ll want to verify that the user has a camera installed. There are two ways to check whether the user has a camera installed:

• Check the static Camera.names property which contains an array of camera names which are available. Typically this array will have one or fewer strings, as most users will not likely have more than one camera installed at a time. The following code demonstrates how you could check the Camera.names property to see if the user has any available cameras:

if (Camera.names.length &gt; 0)

{

trace(&quot;User has at least one camera installed.&quot;);

var cam:Camera = Camera.getCamera(); // Get default camera.

}

else

{

trace(&quot;User has no cameras installed.&quot;);

}

• Check the return value of the static Camera.getCamera() method. If no cameras are available or installed, this method returns null, otherwise it returns a reference to a Camera object. The following code demonstrates how you could check the Camera.getCamera() method to see if the user has any available cameras:

var cam:Camera = Camera.getCamera(); if (cam == null)

{

trace(&quot;User has no cameras installed.&quot;);

}

else

{

trace(&quot;User has at least 1 camera installed.&quot;);

}

Since the Camera class doesn’t extend the DisplayObject class, it cannot be directly added to the display list using the addChild() method. In order to display the camera’s captured video, you need to create a new Video object and call the attachCamera() method on the Video instance.

This snippet shows how you can attach the camera if one exists; if not, the application simply displays nothing:

var cam:Camera = Camera.getCamera(); if (cam != null)

{

var vid:Video = new Video(); vid.attachCamera(cam); addChild(vid);

}

Mobile device cameras

The Camera class is not supported in the Flash Player runtime in mobile browsers.

In AIR applications on mobile devices you can access the camera or cameras on the device. On mobile devices, you can use both the front- and the back-facing camera, but only one camera output can be displayed at any given time. (Attaching a second camera will detach the first.) The front-facing camera is horizontally mirrored on iOS, on Android, it is not.