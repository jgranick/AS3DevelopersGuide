## Monitoring camera status {#monitoring-camera-status}

Flash Player 9 and later, Adobe AIR 1.0 and later

The camera class contains several properties which allow you to monitor the Camera object’s current status. For example, the following code displays several of the camera’s properties using a Timer object and a text field instance on the display list:

var vid:Video;

var cam:Camera = Camera.getCamera(); var tf:TextField = new TextField(); tf.x = 300;

tf.autoSize = TextFieldAutoSize.LEFT; addChild(tf);

if (cam != null)

{

cam.addEventListener(StatusEvent.STATUS, statusHandler); vid = new Video();

vid.attachCamera(cam);

}

function statusHandler(event:StatusEvent):void

{

if (!cam.muted)

{

vid.width = cam.width; vid.height = cam.height; addChild(vid); t.start();

}

cam.removeEventListener(StatusEvent.STATUS, statusHandler);

}

var t:Timer = new Timer(100); t.addEventListener(TimerEvent.TIMER, timerHandler); function timerHandler(event:TimerEvent):void

{

tf.text = &quot;&quot;;

tf.appendText(&quot;activityLevel: &quot; + cam.activityLevel + &quot;\n&quot;); tf.appendText(&quot;bandwidth: &quot; + cam.bandwidth + &quot;\n&quot;); tf.appendText(&quot;currentFPS: &quot; + cam.currentFPS + &quot;\n&quot;); tf.appendText(&quot;fps: &quot; + cam.fps + &quot;\n&quot;); tf.appendText(&quot;keyFrameInterval: &quot; + cam.keyFrameInterval + &quot;\n&quot;); tf.appendText(&quot;loopback: &quot; + cam.loopback + &quot;\n&quot;); tf.appendText(&quot;motionLevel: &quot; + cam.motionLevel + &quot;\n&quot;); tf.appendText(&quot;motionTimeout: &quot; + cam.motionTimeout + &quot;\n&quot;); tf.appendText(&quot;quality: &quot; + cam.quality + &quot;\n&quot;);

}

Every 1/10 of a second (100 milliseconds) the Timer object’s timer event is dispatched and the timerHandler()

function updates the text field on the display list.