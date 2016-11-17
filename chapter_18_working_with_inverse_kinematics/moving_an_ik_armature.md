## Moving an IK Armature {#moving-an-ik-armature}

Flash Player 10 and later, Adobe AIR 1.5 and later, requires Flash CS4 or later

The IKMover moves the axle inside the event listener for the wheel. On each enterFrame event of the wheel, a new target position for the armature is calculated. Using its moveTo() method, the IKMover moves the tail joint to its target position or as far as it can within the constraints set by its limitByDistance, limitByIteration, and limitByTime properties.

Wheel.addEventListener(Event.ENTER_FRAME, frameFunc);

function frameFunc(event:Event)

{

if (Wheel != null)

{

var mat:Matrix = Wheel.transform.matrix; var pt = new Point(90,0);

pt = mat.transformPoint(pt);

ik.moveTo(pt);

}

}