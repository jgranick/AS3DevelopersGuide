## Using IK Events {#using-ik-events}

Flash Player 10 and later, Adobe AIR 1.5 and later, requires Flash CS4 or later

The IKEvent class lets you create an event object that contains information about IK Events. IKEvent information describes motion that has terminated because the specified time, distance, or iteration limit was exceeded.

The following code shows an event listener and handler for tracking time limit events. This event handler reports on the time, distance, iteration count, and joint properties of an event that fires when the time limit of the IKMover is exceeded.

var ikmover:IKMover = new IKMover(endjoint, pos); ikMover.limitByTime = true;

ikMover.timeLimit = 1000; ikmover.addEventListener(IKEvent.TIME_LIMIT, timeLimitFunction);

function timeLimitFunction(evt:IKEvent):void

{

trace(&quot;timeLimit hit&quot;); trace(&quot;time is &quot; + evt.time);

trace(&quot;distance is &quot; + evt.distance); trace(&quot;iterationCount is &quot; + evt.iterationCount); trace(&quot;IKJoint is &quot; + evt.joint.name);

}