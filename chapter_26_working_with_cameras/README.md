# Chapter 26: Working with cameras {#chapter-26-working-with-cameras}

Flash Player 9 and later, Adobe AIR 1.0 and later

A camera attached to a user’s computer can serve as a source of video data that you can display and manipulate using ActionScript. The [Camera](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/media/Camera.html) class is the mechanism built into ActionScript for working with a computer or device camera.

On mobile devices, you can also use the [CameraUI](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/media/CameraUI.html) class. The CameraUI class launches a separate camera application to allow the user to capture a still image or video. When the user is finished, your application can access the image or video through a [MediaPromise](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/media/MediaPromise.html) object.

**More Help topics**

[Christian Cantrell: How to use CameraUI in a Cross-platform Way](http://blogs.adobe.com/cantrell/archives/2011/02/how-to-use-cameraui-in-a-cross-platform-way.html) [Michaël CHAIZE: Android, AIR and the Camera](http://www.riagora.com/2010/07/android-air-and-the-camera/)

**Understanding the Camera class**

Flash Player 9 and later, Adobe AIR 1.0 and later

The Camera object allows you to connect to the user’s local camera and broadcast the video either locally (back to the user) or remotely to a server (such as Flash Media Server).

Using the Camera class, you can access the following kinds of information about the user’s camera:

• Which cameras installed on the user’s computer or device are available

• Whether a camera is installed

• Whether Flash Player is allowed or denied access to the user’s camera

• Which camera is currently active

• The width and height of the video being captured

The Camera class includes several useful methods and properties for working with camera objects. For example, the static Camera.names property contains an array of camera names currently installed on the user’s computer. You can also use the name property to display the name of the currently active camera.

**_Note:_ **_When streaming camera video across the network, you should always handle network interruptions. Network interruptions can occur for many reasons, particularly on mobile devices._