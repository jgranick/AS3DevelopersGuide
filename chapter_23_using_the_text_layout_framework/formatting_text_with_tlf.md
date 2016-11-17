## Formatting text with TLF {#formatting-text-with-tlf}

Flash Player 10 and later, Adobe AIR 1.5 and later

The flashx.textLayout.formats package contains interfaces and classes that allow you to assign formats to any FlowElement in the text flow hierarchy tree. There are two ways to apply formatting. You can assign a specific format individually or assign a group of formats simultaneously with a special formatting object.

The ITextLayoutFormat interface contains all of the formats that can be applied to a FlowElement. Some formats apply to an entire container or paragraph of text, but do not logically apply to individual characters. For example, formats such as justification and tab stops apply to whole paragraphs, but are not applicable to individual characters.

**Assigning formats to a FlowElement with properties**

Flash Player 10 and later, Adobe AIR 1.5 and later

You can set formats on any FlowElement through property assignment. The FlowElement class implements the ITextLayoutFormat interface, so any subclass of the FlowElement class must also implement that interface.

For example, the following code shows how to assign individual formats to an instance of ParagraphElement:

var p:ParagraphElement = new ParagraphElement(); p.fontSize = 18;

p.fontFamily = &quot;Arial&quot;;

### Assigning formats to a FlowElement with the TextLayoutFormat class {#assigning-formats-to-a-flowelement-with-the-textlayoutformat-class}

Flash Player 10 and later, Adobe AIR 1.5 and later

You can apply formats to a FlowElement with the TextLayoutFormat class. You use this class to create a special formatting object that contains all of the formatting values you want. You can then assign that object to the format property of any FlowElement object. Both TextLayoutFormat and FlowElement implement the ITextLayoutFormat interface. This arrangement ensures that both classes contain the same format properties.

For more information, see TextLayoutFormat in the ActionScript 3.0 Reference for the Adobe Flash Platform.

### Format inheritance {#format-inheritance}

Flash Player 10 and later, Adobe AIR 1.5 and later

Formats are inherited through the text flow hierarchy. If you assign an instance of TextLayoutFormat to a FlowElement instance with children, the framework initiates a process called a _cascade_. During a cascade, the framework recursively examines each node in the hierarchy that inherits from your FlowElement. It then determines whether to assign the inherited values to each formatting property. The following rules are applied during the cascade:

1.  Property values are inherited only from an immediate ancestor (sometimes called the parent).
2.  Property values are inherited only if a property does not already have a value (that is, the value is undefined).
3.  Some attributes do not inherit values when undefined, unless the attribute’s value is set to “inherit” or the constant

flashx.textLayout.formats.FormatValue.INHERIT.

For example, if you set the fontSize value at the TextFlow level, the setting applies to all elements in the TextFlow. In other words, the values cascade down the text flow hierarchy. You can, however, override the value in a given element by assigning a new value directly to the element. As a counter-example, if you set the backgroundColor value for at the TextFlow level, the children of the TextFlow do not inherit that value. The backgroundColor property is one that does not inherit from its parent during a cascade. You can override this behavior by setting the backgroundColor property on each child to flashx.textLayout.formats.FormatValue.INHERIT.

For more information, see TextLayoutFormat in the ActionScript 3.0 Reference for the Adobe Flash Platform.