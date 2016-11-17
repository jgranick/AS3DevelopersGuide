# Chapter 18: Working with inverse kinematics {#chapter-18-working-with-inverse-kinematics}

Flash Player 10 and later, Adobe AIR 1.5 and later, requires Flash CS4 or later

Inverse kinematics (IK) is a great technique for creating realistic motion.

IK lets you create coordinated movements within a chain of connected parts called an IK armature, so that the parts move together in a lifelike way. The parts of the armature are its bones and joints. Given the end point of the armature, IK calculates the angles for the joints that are required to reach that end point.

Calculating those angles manually yourself would be challenging. The beauty of this feature is that you can create armatures interactively using Adobe® Flash® Professional. Then animate them using ActionScript. The IK engine included with Flash Professional performs the calculations to describe the movement of the armature. You can limit the movement to certain parameters in your ActionScript code.

New to the Flash Professional CS5 version of IK is the concept of bone spring, typically associated with high-end animation applications. Used with the new dynamic Physics Engine, this feature lets you configure life-like movement. And, this effect is visible both at runtime and during authoring.

To create inverse kinematics armatures, you must have a license for Flash Professional.

**More Help topics**

[fl.ik package](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/fl/ik/package-detail.html)

**Basics of Inverse Kinematics**

Flash Player 10 and later, Adobe AIR 1.5 and later, requires Flash CS4 or later

Inverse kinematics (IK) lets you create life-like animation by linking parts so they move in relation to one another in a realistic manner.

For example, using IK you can move a leg to a certain position by articulating the movements of the joints in the leg required to achieve the desired pose. IK uses a framework of bones chained together in a structure called an IK armature. The fl.ik package helps you create animations resembling natural motion. It lets you animate multiple IK armatures seamlessly without having to know a lot about the physics behind the IK algorithms.

Create the IK armature with its ancillary bones and joints with Flash Professional. Then you can access the IK classes to animate them at runtime.

See the Using inverse kinematics section in _Using Flash Professional_ for detailed instructions on how to create an IK armature.

Important concepts and terms

The following reference list contains important terms that are relevant to this feature:

**Armature** A kinematic chain, consisting of bones and joints, used in computer animation to simulate realistic motion.

**Bone** A rigid segment in an armature, analogous to a bone in an animal skeleton.

**Inverse Kinematics (IK)** Process of determining the parameters of a jointed flexible object called a kinematic chain or armature.

**Joint** The location at which two bones make contact, constructed to enable movement of the bones; analogous to a joint in an animal.

**Physics Engine** A package of physics-related algorithms used to provide life-like actions to animation.

**Spring** The quality of a bone that moves and reacts when the parent bone is moved and then incrementally diminishes over time.