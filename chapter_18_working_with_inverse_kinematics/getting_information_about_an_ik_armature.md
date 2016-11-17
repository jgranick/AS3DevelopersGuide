## Getting information about an IK armature {#getting-information-about-an-ik-armature}

Flash Player 10 and later, Adobe AIR 1.5 and later, requires Flash CS4 or later

First, declare variables for the armature, the bone, and the joint that make up the parts that you want to move.

The following code uses the getArmatureByName() method of the IKManager class to assign the value of the Axle armature to the IKArmature variable tree. The Axle armature was previously created with Flash Professional.

var tree:IKArmature = IKManager.getArmatureByName(&quot;Axle&quot;);

Similarly, the following code uses the getBoneByName() method of the IKArmature class to assign to the IKBone variable the value of the ikBone2 bone.

var bone:IKBone = tree.getBoneByName(&quot;ikBone2&quot;);

The tail joint of the ikBone2 bone is the part of the armature that attaches to the spinning wheel.

The following line declares the variable endEffector and assigns to it the tailjoint property of the ikBone2 bone:

var endEffector:IKJoint = home.tailjoint;

The variable pos is a point that stores the current position of the endEffector joint.

var pos:Point = endEffector.position;

In this example, pos is the position of the joint at the end of the axle where it connects to the wheel. The original value of this variable is obtained from the position property of the IKJoint.