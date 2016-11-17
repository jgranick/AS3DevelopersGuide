## Instantiating an IK Mover and Limiting Its Movement {#instantiating-an-ik-mover-and-limiting-its-movement}

Flash Player 10 and later, Adobe AIR 1.5 and later, requires Flash CS4 or later

An instance of the IKMover class moves the axle.

The following line instantiates the IKMover object ik, passing to its constructor the element to move and the starting point for the movement:

var ik:IKMover = new IKMover(endEffector, pos);

The properties of the IKMover class let you limit the movement of an armature. You can limit movement based on the distance, iterations, and time of the movement.

The following pairs of properties enforce these limits. The pairs consist of a Boolean value that indicates whether the movement is limited and an integer that specifies the limit:

| Boolean property | Integer property | Limit set |
| --- | --- | --- |
| limitByDistance:Boolean | distanceLimit:int | Sets the maximum distance in pixels that the IK engine moves for each iteration. |
| limitByIteration:Boolean | iterationLimit:int | Sets the maximum number of iterations the IK engine performs for each movement. |
| limitByTime:Boolean | timeLimit:int | Sets the maximum time in milliseconds allotted to the IK engine to perform the movement. |

By default, all the Boolean values are set to false, so movement is not limited unless you explicitly set a Boolean value to true. To enforce a limit, set the appropriate property to true and then specify a value for the corresponding integer property. If you set the limit to a value without setting its corresponding Boolean property, the limit is ignored. In this case, the IK engine continues to move the object until another limit or the target position of the IKMover is reached.

In the following example, the maximum distance of the armature movement is set to 0.1 pixels per iteration. The maximum number of iterations for every movement is set to ten.

ik.limitByDistance = true; ik.distanceLimit = 0.1; ik.limitByIteration = true; ik.iterationLimit = 10;