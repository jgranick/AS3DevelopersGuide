## Positioning images within text {#positioning-images-within-text}

To position the InlineGraphicElement within the text, you use the following properties:

*   float property of the InlineGraphicElement class
*   clearFloats property of the FlowElement

The float property controls the placement of the graphic and the text around it. The clearFloats property controls the placement of the paragraph elements relative to the float.

To control the location of an image within a text element, you use the float property. The following example adds an image to a paragraph and aligns it to the left so the text wraps around the right:

&lt;flow:p paragraphSpaceAfter=&quot;15&quot; &gt;Images in a flow are a good thing. For example, here is a float. It should show on the left: &lt;flow:img float=&quot;left&quot; height=&quot;50&quot; width=&quot;19&quot; source=&quot;../assets/bulldog.jpg&quot;&gt;&lt;/flow:img&gt; Don&#039;t you agree? Another sentence here. Another sentence here. Another sentence here. Another sentence here. Another sentence here. Another sentence here. Another sentence here. Another sentence here.&lt;/flow:p&gt;

Valid values for the float property are “left”, “right”, “start”, “end”, and “none”. The Float class defines these constants. The default value is “none”.

The clearFloats property is useful in cases where you want to adjust the starting position of subsequent paragraphs that would normally wrap around the image. For example, assume that you have an image that is larger than the first paragraph. To be sure the second paragraph starts _after_ the image, set the clearFloats property.

The following example uses an image that is taller than the text in the first paragraph. To get the second paragraph to start after the image in the text block, this example sets the clearFloats property on the second paragraph to “end”.

&lt;flow:p paragraphSpaceAfter=&quot;15&quot; &gt;Here is another float, it should show up on the right:

&lt;flow:img float=&quot;right&quot; height=&quot;50&quot; elementHeight=&quot;200&quot; width=&quot;19&quot; source=&quot;../assets/bulldog.jpg&quot;&gt;&lt;/flow:img&gt;We&#039;ll add another paragraph that should clear past it.&lt;/flow:p&gt;&lt;flow:p clearFloats=&quot;end&quot; &gt;This should appear after the previous float on the right.&lt;/flow:p&gt;

Valid values for the clearFloats property are “left”, “right”, “end”, “start”, “none”, and “both”. The ClearFloats class defines these constants. You can also set the clearFloats property to “inherit”, which is a constant defined by the FormatValue class. The default value is “none”.

More Help topics

[TLF Floats](http://blogs.adobe.com/tlf/2010/07/floats.html)