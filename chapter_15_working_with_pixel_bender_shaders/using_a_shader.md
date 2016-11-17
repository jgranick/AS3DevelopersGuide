## Using a shader {#using-a-shader}

Flash Player 10 and later, Adobe AIR 1.5 and later

Once a Pixel Bender shader is available in ActionScript as a Shader object, it can be used in several ways:

*   Shader drawing fill: The shader defines the fill portion of a shape drawn using the drawing api
*   Blend mode: The shader defines the blend between two overlapping display objects
*   Filter: The shader defines a filter that modifies the appearance of visual content
*   Stand-alone shader processing: The shader processing runs without specifying the intended use of the output. The shader can optionally run in the background, with the result is available when the processing completes. This technique can be used to generate bitmap data and also to process non-visual data.

**Using a shader as a drawing fill**

Flash Player 10 and later, Adobe AIR 1.5 and later

When you use a shader to create a drawing fill, you use the drawing api methods to create a vector shape. The shader’s output is used to fill in the shape, in the same way that any bitmap image can be used as a bitmap fill with the drawing api. To create a shader fill, at the point in your code at which you want to start drawing the shape, call the Graphics object’s beginShaderFill() method. Pass the Shader object as the first argument to the beginShaderFill() method, as shown in this listing:

var canvas:Sprite = new Sprite(); canvas.graphics.beginShaderFill(myShader); canvas.graphics.drawRect(10, 10, 150, 150); canvas.graphics.endFill();

// add canvas to the display list to see the result

When you use a shader as a drawing fill, you set any input image values and parameter values that the shader requires.

The following example demonstrates using a shader as a drawing fill. In this example, the shader creates a three-point gradient. This gradient has three colors, each at the point of a triangle, with a gradient blend between them. In addition, the colors rotate to create an animated spinning color effect.

**_Note:_ **_The code for this example was written by Petri Leskinen. Thank you Petri for sharing this example. You can see more of Petri’s examples and tutorials at_ [_http://pixelero.wordpress.com/_](http://pixelero.wordpress.com/)_._

The ActionScript code is in three methods:

*   init(): The init() method is called when the application loads. In this method the code sets the initial values for the Point objects representing the points of the triangle. The also code creates a Sprite instance named canvas. Later, in the updateShaderFill(), the code draws the shader result into canvas once per frame. Finally, the code loads the shader bytecode file.
*   onLoadComplete(): In the onLoadComplete() method the code creates the Shader object named shader. It also sets the initial parameter values. Finally, the code adds the updateShaderFill() method as a listener for the enterFrame event, meaning that it is called once per frame to create an animation effect.
*   updateShaderFill(): The updateShaderFill() method is called once per frame, creating the animation effect. In this method, the code calculates and sets the shader parameters’ values. The code then calls the beginShaderFill() method to create a shader fill and calls other drawing api methods to draw the shader result in a triangle.

The following is the ActionScript code for this example. Use this class as the main application class for an ActionScript- only project in Flash Builder, or as the document class for a FLA file in Flash Professional:

package

{

import flash.display.Shader; import flash.display.Sprite; import flash.events.Event; import flash.geom.Point; import flash.net.URLLoader;

import flash.net.URLLoaderDataFormat; import flash.net.URLRequest;

public class ThreePointGradient extends Sprite

{

private var canvas:Sprite; private var shader:Shader; private var loader:URLLoader;

private var topMiddle:Point; private var bottomLeft:Point; private var bottomRight:Point;

private var colorAngle:Number = 0.0;

private const d120:Number = 120 / 180 * Math.PI; // 120 degrees in radians

public function ThreePointGradient()

{

init();

}

private function init():void

{

canvas = new Sprite(); addChild(canvas);

var size:int = 400;

topMiddle = new Point(size / 2, 10); bottomLeft = new Point(0, size - 10); bottomRight = new Point(size, size - 10);

loader = new URLLoader();

loader.dataFormat = URLLoaderDataFormat.BINARY; loader.addEventListener(Event.COMPLETE, onLoadComplete); loader.load(new URLRequest(&quot;ThreePointGradient.pbj&quot;));

}

private function onLoadComplete(event:Event):void

{

shader = new Shader(loader.data);

shader.data.point1.value = [topMiddle.x, topMiddle.y]; shader.data.point2.value = [bottomLeft.x, bottomLeft.y]; shader.data.point3.value = [bottomRight.x, bottomRight.y];

addEventListener(Event.ENTER_FRAME, updateShaderFill);

}

private function updateShaderFill(event:Event):void

{

colorAngle += .06;

var c1:Number = 1 / 3 + 2 / 3 * Math.cos(colorAngle);

var c2:Number = 1 / 3 + 2 / 3 * Math.cos(colorAngle + d120); var c3:Number = 1 / 3 + 2 / 3 * Math.cos(colorAngle - d120);

shader.data.color1.value = [c1, c2, c3, 1.0]; shader.data.color2.value = [c3, c1, c2, 1.0]; shader.data.color3.value = [c2, c3, c1, 1.0];

canvas.graphics.clear(); canvas.graphics.beginShaderFill(shader);

canvas.graphics.moveTo(topMiddle.x, topMiddle.y); canvas.graphics.lineTo(bottomLeft.x, bottomLeft.y); canvas.graphics.lineTo(bottomRight.x, bottomLeft.y);

canvas.graphics.endFill();

}

}

}

The following is the source code for the ThreePointGradient shader kernel, used to create the “ThreePointGradient.pbj” Pixel Bender bytecode file:

&lt;languageVersion : 1.0;&gt; kernel ThreePointGradient

&lt;

namespace : &quot;Petri Leskinen::Example&quot;; vendor : &quot;Petri Leskinen&quot;;

version : 1;

description : &quot;Creates a gradient fill using three specified points and colors.&quot;;

&gt;

{

parameter float2 point1 // coordinates of the first point

&lt;

minValue:float2(0, 0);

maxValue:float2(4000, 4000);

defaultValue:float2(0, 0);

&gt;;

parameter float4 color1 // color at the first point, opaque red by default

&lt;

defaultValue:float4(1.0, 0.0, 0.0, 1.0);

&gt;;

parameter float2 point2 // coordinates of the second point

&lt;

minValue:float2(0, 0);

maxValue:float2(4000, 4000);

defaultValue:float2(0, 500);

&gt;;

parameter float4 color2 // color at the second point, opaque green by default

&lt;

defaultValue:float4(0.0, 1.0, 0.0, 1.0);

&gt;;

parameter float2 point3 // coordinates of the third point

&lt;

minValue:float2(0, 0);

maxValue:float2(4000, 4000);

defaultValue:float2(0, 500);

&gt;;

parameter float4 color3 // color at the third point, opaque blue by default

&lt;

defaultValue:float4(0.0, 0.0, 1.0, 1.0);

&gt;;

output pixel4 dst;

void evaluatePixel()

{

float2 d2 = point2 - point1; float2 d3 = point3 - point1;

// transformation to a new coordinate system

// transforms point 1 to origin, point2 to (1, 0), and point3 to (0, 1)

float2x2 mtrx = float2x2(d3.y, -d2.y, -d3.x, d2.x) / (d2.x * d3.y - d3.x * d2.y); float2 pNew = mtrx * (outCoord() - point1);

// repeat the edge colors on the outside

pNew.xy = clamp(pNew.xy, 0.0, 1.0); // set the range to 0.0 ... 1.0

// interpolating the output color or alpha value

dst = mix(mix(color1, color2, pNew.x), color3, pNew.y);

}

}

**_Note:_ **_If you use a shader fill when rendering under the graphics processing unit (GPU), the filled area will be colored cyan._

For more information about drawing shapes using the drawing api, see

“Using the drawing API” on page 222

.

### Using a shader as a blend mode {#using-a-shader-as-a-blend-mode}

Flash Player 10 and later, Adobe AIR 1.5 and later

Using a shader as a blend mode is like using other blend modes. The shader defines the appearance resulting from two display objects being blended together visually. To use a shader as a blend mode, assign your Shader object to the blendShader property of the foreground display object. Assigning a value other than null to the blendShader property automatically sets the display object’s blendMode property to BlendMode.SHADER. The following listing demonstrates using a shader as a blend mode. Note that this example assumes that there is a display object named foreground contained in the same parent on the display list as other display content, with foreground overlapping the other content:

foreground.blendShader = myShader;

When you use a shader as a blend mode, the shader must be defined with at least two inputs. As the example shows, you do not set the input values in your code. Instead, the two blended images are automatically used as shader inputs. The foreground image is set as the second image. (This is the display object to which the blend mode is applied.) A background image is created by taking the composite of all the pixels behind the foreground image’s bounding box. This background image is set as the first input image. If you use a shader that expects more than two inputs, you provide a value for any input beyond the first two.

The following example demonstrates using a shader as a blend mode. This example uses a lighten blend mode based on luminosity. The result of the blend is that the lightest pixel value from either of the blended objects becomes the pixel that’s displayed.

**_Note:_ **_The code for this example was written by Mario Klingemann. Thank you Mario for sharing this example. You can see more of Mario’s work and read his writing at_ [_www.quasimondo.com/_](http://www.quasimondo.com/)_._

The important ActionScript code is in these two methods:

*   init(): The init() method is called when the application loads. In this method the code loads the shader bytecode file.
*   onLoadComplete(): In the onLoadComplete() method the code creates the Shader object named shader. It then draws three objects. The first, backdrop, is a dark gray background behind the blended objects. The second, backgroundShape, is a green gradient ellipse. The third object, foregroundShape, is an orange gradient ellipse.

The foregroundShape ellipse is the foreground object of the blend. The background image of the blend is formed by the part of backdrop and the part of backgroundShape that are overlapped by the foregroundShape object’s bounding box. The foregroundShape object is the front-most object in the display list. It partially overlaps backgroundShape and completely overlaps backdrop. Because of this overlap, without a blend mode applied, the orange ellipse (foregroundShape) shows completely and part of the green ellipse (backgroundShape) is hidden by it:

However, with the blend mode applied, the brighter part of the green ellipse “shows through” because it is lighter than the portion of foregroundShape that overlaps it:

The following is the ActionScript code for this example. Use this class as the main application class for an ActionScript- only project in Flash Builder, or as the document class for the FLA file in Flash Professional:

package

{

import flash.display.BlendMode; import flash.display.GradientType; import flash.display.Graphics; import flash.display.Shader; import flash.display.Shape;

import flash.display.Sprite; import flash.events.Event; import flash.geom.Matrix; import flash.net.URLLoader;

import flash.net.URLLoaderDataFormat; import flash.net.URLRequest;

public class LumaLighten extends Sprite

{

private var shader:Shader; private var loader:URLLoader;

public function LumaLighten()

{

init();

}

private function init():void

{

loader = new URLLoader();

loader.dataFormat = URLLoaderDataFormat.BINARY; loader.addEventListener(Event.COMPLETE, onLoadComplete); loader.load(new URLRequest(&quot;LumaLighten.pbj&quot;));

}

private function onLoadComplete(event:Event):void

{

shader = new Shader(loader.data);

var backdrop:Shape = new Shape(); var g0:Graphics = backdrop.graphics; g0.beginFill(0x303030); g0.drawRect(0, 0, 400, 200); g0.endFill();

addChild(backdrop);

var backgroundShape:Shape = new Shape(); var g1:Graphics = backgroundShape.graphics; var c1:Array = [0x336600, 0x80ff00];

var a1:Array = [255, 255];

var r1:Array = [100, 255]; var m1:Matrix = new Matrix();

m1.createGradientBox(300, 200); g1.beginGradientFill(GradientType.LINEAR, c1, a1, r1, m1); g1.drawEllipse(0, 0, 300, 200);

g1.endFill(); addChild(backgroundShape);

var foregroundShape:Shape = new Shape(); var g2:Graphics = foregroundShape.graphics; var c2:Array = [0xff8000, 0x663300];

var a2:Array = [255, 255];

var r2:Array = [100, 255]; var m2:Matrix = new Matrix();

m2.createGradientBox(300, 200); g2.beginGradientFill(GradientType.LINEAR, c2, a2, r2, m2); g2.drawEllipse(100, 0, 300, 200);

g2.endFill(); addChild(foregroundShape);

foregroundShape.blendShader = shader; foregroundShape.blendMode = BlendMode.SHADER;

}

}

}

The following is the source code for the LumaLighten shader kernel, used to create the “LumaLighten.pbj” Pixel Bender bytecode file:

&lt;languageVersion : 1.0;&gt; kernel LumaLighten

&lt;

namespace : &quot;com.quasimondo.blendModes&quot;; vendor : &quot;Quasimondo.com&quot;;

version : 1;

description : &quot;Luminance based lighten blend mode&quot;;

&gt;

{

input image4 background; input image4 foreground;

output pixel4 dst;

const float3 LUMA = float3(0.212671, 0.715160, 0.072169);

void evaluatePixel()

{

float4 a = sampleNearest(foreground, outCoord()); float4 b = sampleNearest(background, outCoord());

float luma_a = a.r * LUMA.r + a.g * LUMA.g + a.b * LUMA.b; float luma_b = b.r * LUMA.r + b.g * LUMA.g + b.b * LUMA.b;

dst = luma_a &gt; luma_b ? a : b;

}

}

For more information on using blend modes, see

“Applying blending modes” on page 186

.

**_Note:_ **_When a Pixel Bender shader program is run as a blend in Flash Player or AIR, the sampling and outCoord() functions behave differently than in other contexts.In a blend, a sampling function will always return the current pixel being evaluated by the shader. You cannot, for example, use add an offset to outCoord() in order to sample a neighboring pixel. Likewise, if you use the outCoord() function outside a sampling function, its coordinates always evaluate to 0\. You cannot, for example, use the position of a pixel to influence how the blended images are combined._

### Using a shader as a filter {#using-a-shader-as-a-filter}

Flash Player 10 and later, Adobe AIR 1.5 and later

Using a shader as a filter is like using any of the other filters in ActionScript. When you use a shader as a filter, the filtered image (a display object or BitmapData object) is passed to the shader. The shader uses the input image to create the filter output, which is usually a modified version of the original image. If the filtered object is a display object the shader’s output is displayed on the screen in place of the filtered display object. If the filtered object is a BitmapData object, the shader’s output becomes the content of the BitmapData object whose applyFilter() method is called.

To use a shader as a filter, you first create the Shader object as described in

“Loading or embedding a shader” on

page 302

. Next you create a ShaderFilter object linked to the Shader object. The ShaderFilter object is the filter that is applied to the filtered object. You apply it to an object in the same way that you apply any filter. You pass it to the filters property of a display object or you call the applyFilter() method on a BitmapData object. For example, the following code creates a ShaderFilter object and applies the filter to a display object named homeButton.

var myFilter:ShaderFilter = new ShaderFilter(myShader); homeButton.filters = [myFilter];

When you use a shader as a filter, the shader must be defined with at least one input. As the example shows, you do not set the input value in your code. Instead, the filtered display object or BitmapData object is set as the input image. If you use a shader that expects more than one input, you provide a value for any input beyond the first one.

In some cases, a filter changes the dimensions of the original image. For example, a typical drop shadow effect adds extra pixels containing the shadow that’s added to the image. When you use a shader that changes the image dimensions, set the leftExtension, rightExtension, topExtension, and bottomExtension properties to indicate by how much you want the image size to change.

The following example demonstrates using a shader as a filter. The filter in this example inverts the red, green, and blue channel values of an image. The result is the “negative” version of the image.

**_Note:_ **_The shader that this example uses is the invertRGB.pbk Pixel Bender kernel that is included with the Pixel Bender Toolkit. You can load the source code for the kernel from the Pixel Bender Toolkit installation directory. Compile the source code and save the bytecode file in the same directory as the source code._

The important ActionScript code is in these two methods:

*   init(): The init() method is called when the application loads. In this method the code loads the shader bytecode file.
*   onLoadComplete(): In the onLoadComplete() method the code creates the Shader object named shader. It then creates and draws the contents of an object named target. The target object is a rectangle filled with a linear gradient color that is red on the left, yellow-green in the middle, and light blue on the right. The unfiltered object looks like this:

With the filter applied the colors are inverted, making the rectangle look like this:

The shader that this example uses is the “invertRGB.pbk” sample Pixel Bender kernel that is included with the Pixel Bender Toolkit. The source code is available in the file “invertRGB.pbk” in the Pixel Bender Toolkit installation directory. Compile the source code and save the bytecode file with the name “invertRGB.pbj” in the same directory as your ActionScript source code.

The following is the ActionScript code for this example. Use this class as the main application class for an ActionScript- only project in Flash Builder, or as the document class for the FLA file in Flash Professional:

package

{

import flash.display.GradientType; import flash.display.Graphics; import flash.display.Shader; import flash.display.Shape;

import flash.display.Sprite; import flash.filters.ShaderFilter; import flash.events.Event;

import flash.geom.Matrix; import flash.net.URLLoader;

import flash.net.URLLoaderDataFormat; import flash.net.URLRequest;

public class InvertRGB extends Sprite

{

private var shader:Shader; private var loader:URLLoader;

public function InvertRGB()

{

init();

}

private function init():void

{

loader = new URLLoader();

loader.dataFormat = URLLoaderDataFormat.BINARY; loader.addEventListener(Event.COMPLETE, onLoadComplete); loader.load(new URLRequest(&quot;invertRGB.pbj&quot;));

}

private function onLoadComplete(event:Event):void

{

shader = new Shader(loader.data);

var target:Shape = new Shape(); addChild(target);

var g:Graphics = target.graphics;

var c:Array = [0x990000, 0x445500, 0x007799];

var a:Array = [255, 255, 255];

var r:Array = [0, 127, 255]; var m:Matrix = new Matrix(); m.createGradientBox(w, h);

g.beginGradientFill(GradientType.LINEAR, c, a, r, m); g.drawRect(10, 10, w, h);

g.endFill();

var invertFilter:ShaderFilter = new ShaderFilter(shader); target.filters = [invertFilter];

}

}

}

For more information on applying filters, see

“Creating and applying filters” on page 268

.

### Using a shader in stand-alone mode {#using-a-shader-in-stand-alone-mode}

Flash Player 10 and later, Adobe AIR 1.5 and later

When you use a shader in stand-alone mode, the shader processing runs independent of how you intend to use the output. You specify a shader to execute, set input and parameter values, and designate an object into which the result data is placed. You can use a shader in stand-alone mode for two reasons:

*   Processing non-image data: In stand-alone mode, you can choose to pass arbitrary binary or number data to the shader rather than bitmap image data. You can choose to have the shader result be returned as binary data or number data in addition to bitmap image data.
*   Background processing: When you run a shader in stand-alone mode, by default the shader executes asynchronously. This means that the shader runs in the background while your application continues to run, and your code is notified when the shader processing finishes. You can use a shader that takes a long time to run and it doesn’t freeze up the application user interface or other processing while the shader is running.

You use a ShaderJob object to execute a shader in stand-alone mode. First you create the ShaderJob object and link it to the Shader object representing the shader to execute:

var job:ShaderJob = new ShaderJob(myShader);

Next, you set any input or parameter values that the shader expects. If you are executing the shader in the background, you also register a listener for the ShaderJob object’s complete event. Your listener is called when the shader finishes its work:

function completeHandler(event:ShaderEvent):void

{

// do something with the shader result

}

job.addEventListener(ShaderEvent.COMPLETE, completeHandler);

Next, you create an object into which the shader operation result is written when the operation finishes. You assign that object to the ShaderJob object’s target property:

var jobResult:BitmapData = new BitmapData(100, 75); job.target = jobResult;

Assign a BitmapData instance to the target property if you are using the ShaderJob to perform image processing. If you are processing binary or number data, assign a ByteArray object or Vector.&lt;Number&gt; instance to the target property. In that case, you must set the ShaderJob object’s width and height properties to specify the amount of data to output to the target object.

**_Note:_ **_You can set the ShaderJob object’s shader, target,width, and height properties in one step by passing arguments to the ShaderJob() constructor, like this:var job:ShaderJob = new ShaderJob(myShader, myTarget, myWidth, myHeight);_

When you are ready to execute the shader, you call the ShaderJob object’s start() method:

job.start();

By default calling start() causes the ShaderJob to execute asynchronously. In that case program execution continues immediately with the next line of code rather than waiting for the shader to finish. When the shader operation finishes, the ShaderJob object calls its complete event listeners, notifying them that it is done. At that point (that is, in the body of your complete event listener) the target object contains the shader operation result.

**_Note:_ **_Instead of using the target property object, you can retrieve the shader result directly from the event object that’s passed to your listener method. The event object is a ShaderEvent instance. The ShaderEvent object has three properties that can be used to access the result, depending on the data type of the object you set as the target property: ShaderEvent.bitmapData, ShaderEvent.byteArray, and ShaderEvent.vector._

Alternatively, you can pass a true argument to the start() method. In that case the shader operation executes synchronously. All code (including interaction with the user interface and any other events) pauses while the shader executes. When the shader finishes, the target object contains the shader result and the program continues with the next line of code.

job.start(true);