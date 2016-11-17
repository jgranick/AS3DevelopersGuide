## Handling Events in FTE {#handling-events-in-fte}

Flash Player 10 and later, Adobe AIR 1.5 and later

You can add event listeners to a TextLine instance just as you can to other display objects. For example, you can detect when a user rolls the mouse over a text line or a user clicks the line. The following example detects both of these events. When you roll the mouse over the line, the cursor changes to a button cursor and when you click the line, it changes color.

package

{

import flash.text.engine.*; import flash.ui.Mouse; import flash.display.Sprite

import flash.events.MouseEvent; import flash.events.EventDispatcher;

public class EventHandlerExample extends Sprite

{

var textBlock:TextBlock = new TextBlock();

public function EventHandlerExample():void

{

var str:String = &quot;I&#039;ll change color if you click me.&quot;;

var fontDescription:FontDescription = new FontDescription(&quot;Arial&quot;); var format:ElementFormat = new ElementFormat(fontDescription, 18); var textElement = new TextElement(str, format);

textBlock.content = textElement; createLine(textBlock);

}

private function createLine(textBlock:TextBlock):void

{

var textLine:TextLine = textBlock.createTextLine(null, 500); textLine.x = 30;

textLine.y = 30; addChild(textLine);

textLine.addEventListener(&quot;mouseOut&quot;, mouseOutHandler); textLine.addEventListener(&quot;mouseOver&quot;, mouseOverHandler); textLine.addEventListener(&quot;click&quot;, clickHandler);

}

private function mouseOverHandler(event:MouseEvent):void

{

Mouse.cursor = &quot;button&quot;;

}

private function mouseOutHandler(event:MouseEvent):void

{

Mouse.cursor = &quot;arrow&quot;;

}

function clickHandler(event:MouseEvent):void { if(textBlock.firstLine)

removeChild(textBlock.firstLine);

var newFormat:ElementFormat = textBlock.content.elementFormat.clone();

switch(newFormat.color)

{

case 0x000000:

newFormat.color = 0xFF0000; break;

case 0xFF0000:

newFormat.color = 0x00FF00; break;

case 0x00FF00:

newFormat.color = 0x0000FF; break;

case 0x0000FF:

newFormat.color = 0x000000; break;

}

textBlock.content.elementFormat = newFormat; createLine(textBlock);

}

}

}

**Mirroring events**

Flash Player 10 and later, Adobe AIR 1.5 and later

You can also mirror events on a text block, or on a portion of a text block, to an event dispatcher. First, create an EventDispatcher instance and then assign it to the eventMirror property of a TextElement instance. If the text block consists of a single text element, the text engine mirrors events for the entire text block. If the text block consists of multiple text elements, the text engine mirrors events only for the TextElement instances that have the eventMirror property set. The text in the following example consists of three elements: the word &quot;Click&quot;, the word &quot;here&quot;, and the string &quot;to see me in italic&quot;. The example assigns an event dispatcher to the second text element, the word &quot;here&quot;, and adds an event listener, the clickHandler() method. The clickHandler() method changes the text to italic. It also replaces the content of the third text element to read, &quot;Click here to see me in normal font!&quot;.

package

{

import flash.text.engine.*; import flash.ui.Mouse; import flash.display.Sprite;

import flash.events.MouseEvent; import flash.events.EventDispatcher;

public class EventMirrorExample extends Sprite

{

var fontDescription:FontDescription = new FontDescription(&quot;Helvetica&quot;, &quot;bold&quot;); var format:ElementFormat = new ElementFormat(fontDescription, 18);

var textElement1 = new TextElement(&quot;Click &quot;, format); var textElement2 = new TextElement(&quot;here &quot;, format);

var textElement3 = new TextElement(&quot;to see me in italic! &quot;, format); var textBlock:TextBlock = new TextBlock();

public function EventMirrorExample()

{

var myEvent:EventDispatcher = new EventDispatcher();

myEvent.addEventListener(&quot;click&quot;, clickHandler); myEvent.addEventListener(&quot;mouseOut&quot;, mouseOutHandler); myEvent.addEventListener(&quot;mouseOver&quot;, mouseOverHandler);

textElement2.eventMirror=myEvent;

var groupVector:Vector.&lt;ContentElement&gt; = new Vector.&lt;ContentElement&gt;; groupVector.push(textElement1, textElement2, textElement3);

var groupElement:GroupElement = new GroupElement(groupVector);

textBlock.content = groupElement; createLines(textBlock);

}

private function clickHandler(event:MouseEvent):void

{

var newFont:FontDescription = new FontDescription(); newFont.fontWeight = &quot;bold&quot;;

var newFormat:ElementFormat = new ElementFormat(); newFormat.fontSize = 18;

if(textElement3.text == &quot;to see me in italic! &quot;) { newFont.fontPosture = FontPosture.ITALIC; textElement3.replaceText(0,21, &quot;to see me in normal font! &quot;);

}

else {

newFont.fontPosture = FontPosture.NORMAL; textElement3.replaceText(0, 26, &quot;to see me in italic! &quot;);

}

newFormat.fontDescription = newFont; textElement1.elementFormat = newFormat; textElement2.elementFormat = newFormat; textElement3.elementFormat = newFormat; createLines(textBlock);

}

private function mouseOverHandler(event:MouseEvent):void

{

Mouse.cursor = &quot;button&quot;;

}

private function mouseOutHandler(event:MouseEvent):void

{

Mouse.cursor = &quot;arrow&quot;;

}

private function createLines(textBlock:TextBlock):void

{

if(textBlock.firstLine)

removeChild (textBlock.firstLine);

var textLine:TextLine = textBlock.createTextLine (null, 300); textLine.x = 15;

textLine.y = 20; addChild (textLine);

}

}

}

The mouseOverHandler() and mouseOutHandler() functions set the cursor to a button cursor when it&#039;s over the word &quot;here&quot; and back to an arrow when it&#039;s not.