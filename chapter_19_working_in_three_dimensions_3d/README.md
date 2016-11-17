# Chapter 19: Working in three dimensions (3D) {#chapter-19-working-in-three-dimensions-3d}

Flash Player 10 and later, Adobe AIR 1.5 and later

The Flash Player and AIR runtimes support 3D graphics in two ways. You can use three-dimensional display objects on the Flash display list. This is appropriate for adding three-dimensional effects to Flash content and for low polygon- count objects. In Flash Player 11and AIR 3, or later, you can render complex 3D scenes using the Stage3D API.

A Stage3D viewport is not a display object. Instead, the 3D graphics are rendered to a viewport that is displayed underneath the Flash display list (and above any StageVideo viewport planes). Rather than using the Flash DisplayObject classes to create a scene, you use a programmable 3D-pipeline (similar to OpenGL and Direct3D). This pipeline takes triangle data and textures as input and renders the scene using shader programs that you provide.

Hardware acceleration is used when a compatible graphics processing unit (GPU) with supported drivers, is available on the client computer.

[Stage3D](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/Stage3D.html) provides a very low-level API. In an application, you are encouraged to use a 3D framework that supports Stage3D. You can create your own framework or use one of several commercial and open source frameworks already available.

For more information about developing 3D applications using Stage3D and about available 3D frameworks, visit the [Flash Player Developer Center: Stage 3D](http://goo.gl/hlzhB).

**Basics of 3D display objects**

Flash Player 10 and later, Adobe AIR 1.5 and later

The main difference between a two-dimensional (2D) object and a three-dimensional (3D) object projected on a two- dimensional screen is the addition of a third dimension to the object. The third dimension allows the object to move toward and away from viewpoint of the user.

When you explicitly set the z property of a display object to a numeric value, the object automatically creates a 3D transformation matrix. You can alter this matrix to modify the 3D transformation settings of that object.

In addition, 3D rotation differs from 2D rotation. In 2D the axis of rotation is always perpendicular to the x/y plane - in other words, on the z-axis. In 3D the axis of rotation can be around any of the x, y, or z axes. Setting the rotation and scaling properties of a display object enable it to move in 3D space.

Important concepts and terms

The following reference list contains important terms that you will encounter when programming 3-dimensional graphics:

**Perspective** In a 2D plane, representation of parallel lines as converging on a vanishing point to give the illusion of depth and distance.

**Projection** The production of a 2D image of a higher-dimensional object; 3D projection maps 3D points to a 2D plane.

**Rotation** Changing the orientation (and often the position) of an object by moving every point included in the object in a circular motion.

**Transformation** Altering 3D points or sets of points by translation, rotation, scale, skew, or a combination of these actions.

**Translation** Changing the position of an object by moving every point included in the object by the same amount in the same direction.

**Vanishing point** Point at which receding parallel lines seem to meet when represented in linear perspective.

**Vector** A 3D vector represents a point or a location in the three-dimensional space using the Cartesian coordinates x, y, and z.

**Vertex** A corner point.

**Textured mesh** Any point defining an object in 3D space.

**UV mapping** A way to apply a texture or bitmap to a 3D surface. UV mapping assigns values to coordinates on an image as percentages of the horizontal (U) axis and vertical (V) axis.

**T value** The scaling factor for determining the size of a 3D object as the object moves toward, or away from, the current point of view.

**Culling** Rendering, or not, surfaces with specific winding. Using culling you can hide surfaces that are not visible to the current point of view.