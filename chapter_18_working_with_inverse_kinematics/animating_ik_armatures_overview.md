## Animating IK Armatures Overview {#animating-ik-armatures-overview}

Flash Player 10 and later, Adobe AIR 1.5 and later, requires Flash CS4 or later

After creating an IK armature in Flash Professional, use the fl.ik classes to limit its movement, track its events, and animate it at runtime.

The following figure shows a movie clip named Wheel. The axle is an instance of an IKArmature named Axle. The IKMover class moves the armature in synchronization with the rotation of wheel. The IKBone, ikBone2, in the armature is attached to the wheel at its tail joint.

**A**

**_A._ **_Wheel_ **_B._ **_Axle_ **_C._ **_ikBone2_

At runtime, the wheel spins in association with the motion_Wheel motion tween discussed in

“Describing the

animation” on page 338

. An IKMover object initiates and controls the movement of the axle. The following figure shows two snapshots of the axle armature attached to the spinning wheel at different frames in the rotation.

At runtime, the following ActionScript:

*   Gets information about the armature and its components
*   Instantiates an IKMover object
*   Moves the axle in conjunction with the rotation of the wheel

import fl.ik.*

var tree:IKArmature = IKManager.getArmatureByName(&quot;Axle&quot;); var bone:IKBone = tree.getBoneByName(&quot;ikBone2&quot;);

var endEffector:IKJoint = bone.tailJoint; var pos:Point = endEffector.position;

var ik:IKMover = new IKMover(endEffector, pos); ik.limitByDistance = true;

ik.distanceLimit = 0.1; ik.limitByIteration = true; ik.iterationLimit = 10;

Wheel.addEventListener(Event.ENTER_FRAME, frameFunc); function frameFunc(event:Event)

{

if (Wheel != null)

{

var mat:Matrix = Wheel.transform.matrix; var pt = new Point(90, 0);

pt = mat.transformPoint(pt);

ik.moveTo(pt);

}

}

The IK classes used to move the axle are:

*   IKArmature: describes the armature, a tree structure consisting of bones and joints; must be created with Flash Professional
*   IKManager: container class for all the IK armatures in the document; must be created with Flash Professional
*   IKBone: a segment of an IK armature
*   IKJoint: a connection between two IK bones
*   IKMover: initiates and controls IK movement of armatures

For complete and detailed descriptions of these classes, see the [ik package](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/fl/ik/package-detail.html).