## Connecting to a user’s camera {#connecting-to-a-user-s-camera}

Flash Player 9 and later, Adobe AIR 1.0 and later

The first step when connecting to a user’s camera is to create a new camera instance by creating a variable of type Camera and initializing it to the return value of the static Camera.getCamera() method.

The next step is to create a new video object and attach the Camera object to it.

The third step is to add the video object to the display list. You need to perform steps 2 and 3 because the Camera class does not extend the DisplayObject class so it cannot be added directly to the display list. To display the camera’s captured video, you create a new video object and call the attachCamera() method.

The following code shows these three steps:

var cam:Camera = Camera.getCamera(); var vid:Video = new Video(); vid.attachCamera(cam); addChild(vid);

Note that if a user does not have a camera installed, the application does not display anything.

In real life, you need to perform additional steps for your application. See

“Verifying that cameras are installed” on

page 523

and

“Detecting permissions for camera access” on page 524

for further information.

**More Help topics**

[Lee Brimelow: How to access the camera on Android devices](http://www.gotoandlearn.com/play.php?id=124)