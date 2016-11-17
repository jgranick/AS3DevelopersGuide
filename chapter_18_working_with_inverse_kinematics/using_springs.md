## Using Springs {#using-springs}

Flash Player 10 and later, Adobe AIR 1.5 and later, requires Flash CS5 or later

Inverse kinematics in Flash Professional CS5 supports bone spring. Bone spring can be set during authoring, and bone spring attributes can be added or modified at runtime. Spring is a property of a bone and its joints. It has two attributes: IKJoint.springStrength, which sets the amount of spring, and IKJoint.springDamping, which adds resistance to the strength value and changes the rate of decay of the spring.

Spring strength is a percent value from the default 0 (completely rigid) to 100 (very loose and controlled by physics). Bones with spring react to the movement of their joint. If no other translation (rotation, x, or y) is enabled, the spring settings have no effect.

Spring damping is a percent value from the default 0 (no resistance) to 100 (heavily damped). Damping changes the amount of time between a bone’s initial movement and its return to a rest position.

You can check to see if springs are enabled for an IKArmature object by checking its IKArmature.springsEnabled property. The other spring properties and methods belong to individual IKJoint objects. A joint can be enabled for angular rotation and translation along the x- and y-axes. You can position a rotational joint’s spring angle with IKJoint.setSpringAngle and a translational joint’s spring position with IKJoint.setSpringPt.

This example selects a bone by name and identifies its tailJoint. The code tests the parent armature to see if springs are enabled and then sets spring properties for the joint.

var arm:IKArmature = IKManager.getArmatureAt(0); var bone:IKBone = arm.getBoneByName(&quot;c&quot;);

var joint:IKJoint = bone.tailJoint; if (arm.springsEnabled) {

joint.springStrength = 50; //medium spring strength joint.springDamping = 10; //light damping resistance if (joint.hasSpringAngle) {

joint.setSpringAngle(30); //set angle for rotational spring

}

}