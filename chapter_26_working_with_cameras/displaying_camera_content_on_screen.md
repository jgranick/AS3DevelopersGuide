## Displaying camera content on screen {#displaying-camera-content-on-screen}

Flash Player 9 and later, Adobe AIR 1.0 and later

Connecting to a camera can require less code than using the NetConnection and NetStream classes to load a video. The camera class can also quickly become tricky because with Flash Player, you need a user’s permission to connect to their camera before you can access it.

The following code demonstrates how you can use the Camera class to connect to a user’s local camera:

var cam:Camera = Camera.getCamera(); var vid:Video = new Video(); vid.attachCamera(cam); addChild(vid);

**_Note:_ **_The Camera class does not have a constructor method. In order to create a new Camera instance you use the static_

_Camera.getCamera() method._