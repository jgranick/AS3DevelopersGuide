## Structuring text with TLF {#structuring-text-with-tlf}

The TLF uses a hierarchical tree to represent text. Each node in the tree is an instance of a class defined in the elements package. For example, the root node of the tree is always an instance of the TextFlow class. The TextFlow class represents an entire story of text. A story is a collection of text and other elements that is treated as one unit, or flow. A single story can require more than one column or text container to display.

Apart from the root node, the remaining elements are loosely based on XHTML elements. The following diagram shows the hierarchy of the framework:

_TextFlow Hierarchy_

### Text Layout Framework markup {#text-layout-framework-markup}

Understanding the structure of the TLF is also helpful when dealing with TLF Markup. TLF Markup is an XML representation of text that is part of the TLF. Although the framework also supports other XML formats, TLF Markup is unique in that it is based specifically on the structure of the TextFlow hierarchy. If you export XML from a TextFlow using this markup format, the XML is exported with this hierarchy intact.

TLF Markup provides the highest fidelity representation of text in a TextFlow hierarchy. The markup language provides tags for each of the TextFlow hierarchy’s basic elements, and also provides attributes for all formatting properties available in the TextLayoutFormat class.

The following table contains the tags that can be used in TLF Markup.

| **Element** | **Description** | **Children** | **Class** |
| --- | --- | --- | --- |
| textflow | The root element of the markup. | div, p | TextFlow |
| div | A division within a TextFlow. May contain a group of paragraphs. | div, list, p | DivElement |
| p | A paragraph. | a, tcy, span, img, tab, br, g | ParagraphElement |
| a | A link. | tcy, span, img, tab, br, g | LinkElement |
| tcy | A run of horizontal text (used in a vertical TextFlow). | a, span, img, tab, br, g | TCYElement |
| span | A run of text within a paragraph. |  | SpanElement |
| img | An image in a paragraph. |  | InlineGraphicElement |
| tab | A tab character. |  | TabElement |
| br | A break character. Used for ending a line within a paragraph; text continues on the next line, but remains in the same paragraph. |  | BreakElement |
| linkNormalFormat | Defines the formatting attributes used for links in normal state. | TextLayoutFormat | TextLayoutFormat |
| linkActiveFormat | Defines the formatting attributes used for links in active state, when the mouse is down on a link. | TextLayoutFormat | TextLayoutFormat |
| linkHoverFormat | Defines the formatting attributes used for links in hover state, when the mouse is within the bounds (rolling over) a link. | TextLayoutFormat | TextLayoutFormat |
| li | A list item element. Must be inside a list element. | div, li, list, p | ListItemElement |
| list | A list. Lists can be nested, or placed adjacent to each other. Different labeling or numbering schemes can be applied to the list items. | div, li, list, p | ListElement |
| g | A group element. Used for grouping elements in a paragraph. The lets you nest elements below the paragraph level. | a, tcy, span, img, tab, br, g | SubParagraphGroupE lement |

More Help topics

[TLF 2.0 Lists Markup](http://blogs.adobe.com/tlf/2010/07/tlf-20-lists-markup.html)

[TLF 2.0 SubParagraphGroupElements and typeName](http://blogs.adobe.com/tlf/2011/01/tlf-2-0-changes-subparagraphgroupelements-and-typename-applied-to-textfieldhtmlimporter-and-cssformatresolver.html)

### Using numbered and bulleted lists {#using-numbered-and-bulleted-lists}

You can use the ListElement and ListItemElement classes to add bulleted lists to your text controls. The bulleted lists can be nested and can be customized to use different bullets (or markers) and auto-numbering, as well as outline-style numbering.

To create lists in your text flows, use the &lt;list&gt; tag. You then use &lt;li&gt; tags within the &lt;list&gt; tag for each list item in the list. You can customize the appearance of the bullets by using the ListMarkerFormat class.

The following example creates simple lists:

&lt;flow:list paddingRight=&quot;24&quot; paddingLeft=&quot;24&quot;&gt;

&lt;flow:li&gt;Item 1&lt;/flow:li&gt;

&lt;flow:li&gt;Item 2&lt;/flow:li&gt;

&lt;flow:li&gt;Item 3&lt;/flow:li&gt;

&lt;/flow:list&gt;

You can nest lists within other lists, as the following example shows:

&lt;flow:list paddingRight=&quot;24&quot; paddingLeft=&quot;24&quot;&gt;

&lt;flow:li&gt;Item 1&lt;/flow:li&gt;

&lt;flow:list paddingRight=&quot;24&quot; paddingLeft=&quot;24&quot;&gt;

&lt;flow:li&gt;Item 1a&lt;/flow:li&gt;

&lt;flow:li&gt;Item 1b&lt;/flow:li&gt;

&lt;flow:li&gt;Item 1c&lt;/flow:li&gt;

&lt;/flow:list&gt;

&lt;flow:li&gt;Item 2&lt;/flow:li&gt;

&lt;flow:li&gt;Item 3&lt;/flow:li&gt;

&lt;/flow:list&gt;

To customize the type of marker in the list, use the listStyleType property of the ListElement. This property can be any value defined by the ListStyleType class (such as check, circle, decimal, and box). The following example creates lists with various marker types and a custom counter increment:

&lt;flow:list paddingRight=&quot;24&quot; paddingLeft=&quot;24&quot; listStyleType=&quot;upperAlpha&quot;&gt;

&lt;flow:li&gt;upperAlpha item&lt;/flow:li&gt; &lt;flow:li&gt;another&lt;/flow:li&gt; &lt;/flow:list&gt; &lt;flow:list paddingRight=&quot;24&quot; paddingLeft=&quot;24&quot; listStyleType=&quot;lowerAlpha&quot;&gt; &lt;flow:li&gt;lowerAlpha item&lt;/flow:li&gt; &lt;flow:li&gt;another&lt;/flow:li&gt; &lt;/flow:list&gt; &lt;flow:list paddingRight=&quot;24&quot; paddingLeft=&quot;24&quot; listStyleType=&quot;upperRoman&quot;&gt; &lt;flow:li&gt;upperRoman item&lt;/flow:li&gt;

&lt;flow:li&gt;another&lt;/flow:li&gt; &lt;/flow:list&gt; &lt;flow:list paddingRight=&quot;24&quot; paddingLeft=&quot;24&quot; listStyleType=&quot;lowerRoman&quot;&gt; &lt;flow:listMarkerFormat&gt; &lt;!-- Increments the list by 2s rather than 1s. --&gt; &lt;flow:ListMarkerFormat counterIncrement=&quot;ordered 2&quot;/&gt; &lt;/flow:listMarkerFormat&gt;

&lt;flow:li&gt;lowerRoman item&lt;/flow:li&gt; &lt;flow:li&gt;another&lt;/flow:li&gt; &lt;/flow:list&gt;

You use the ListMarkerFormat class to define the counter. In addition to defining the increment of a counter, you can also customize the counter by resetting it with the counterReset property.

You can further customize the appearance of the markers in your lists by using the beforeContent and afterContent properties of the ListMarkerFormat. These properties apply to content that appears before and after the content of the marker.

The following example adds the string “XX” before the marker, and the string “YY” after the marker:

&lt;flow:list listStyleType=&quot;upperRoman&quot; paddingLeft=&quot;36&quot; paddingRight=&quot;24&quot;&gt;

&lt;flow:listMarkerFormat&gt;

&lt;flow:ListMarkerFormat fontSize=&quot;16&quot; beforeContent=&quot;XX&quot; afterContent=&quot;YY&quot; counterIncrement=&quot;ordered -1&quot;/&gt;

&lt;/flow:listMarkerFormat&gt;

&lt;flow:li&gt;Item 1&lt;/flow:li&gt;

&lt;flow:li&gt;Item 2&lt;/flow:li&gt;

&lt;flow:li&gt;Item 3&lt;/flow:li&gt;

&lt;/flow:list&gt;

The content property itself can define further customizations of the marker format. The following example displays an ordered, uppercase Roman numeral marker:

&lt;flow:list listStyleType=&quot;disc&quot; paddingLeft=&quot;96&quot; paddingRight=&quot;24&quot;&gt;

&lt;flow:listMarkerFormat&gt;

&lt;flow:ListMarkerFormat fontSize=&quot;16&quot; beforeContent=&quot;Section &quot; content=&quot;counters(ordered,&amp;quot;*&amp;quot;,upperRoman)&quot; afterContent=&quot;: &quot;/&gt;

&lt;/flow:listMarkerFormat&gt;

&lt;flow:li&gt;Item 1&lt;/li&gt;

&lt;flow:li&gt;Item 2&lt;/li&gt;

&lt;flow:li&gt;Item 3&lt;/li&gt;

&lt;/flow:list&gt;

As the previous example shows, the content property can also insert a suffix: a string that appears after the marker, but before the afterContent. To insert this string when providing XML content to the flow, wrap the string in &amp;quote; HTML entities rather than quotation marks (&quot;&lt;_string_&gt;&quot;).

More Help topics

[TLF 2.0 Lists Markup](http://blogs.adobe.com/tlf/2010/07/tlf-20-lists-markup.html)

### Using padding in TLF {#using-padding-in-tlf}

Each FlowElement supports padding properties that you use to control the position of each element’s content area, and the space between the content areas.

The total width of an element is the sum of its content’s width, plus the paddingLeft and paddingRight properties. The total height of an element is the sum of its content’s height, plus the paddingTop and paddingBottom properties.

The padding is the space between the border and the content. The padding properties are paddingBottom, paddingTop, paddingLeft, and paddingRight. Padding can be applied to the TextFlow object, as well as the following child elements:

*   div
*   img
*   li
*   list
*   p

Padding properties cannot be applied to span elements.

The following example sets padding properties on the TextFlow:

&lt;flow:TextFlow version=&quot;2.0.0&quot; [xmlns:flow=&quot;http://ns.adobe.com/textLayout/2008&quot;](http://ns.adobe.com/textLayout/2008) fontSize=&quot;14&quot; textIndent=&quot;15&quot; paddingTop=&quot;4&quot; paddingLeft=&quot;4&quot; fontFamily=&quot;Times New Roman&quot;&gt;

Valid values for the padding properties are a number (in pixels), “auto”, or “inherit”. The default value is “auto”, which means it is calculated automatically and set to 0, for all elements except the ListElement. For ListElements, “auto” is 0 except on the start side of the list where the value of the listAutoPadding property is used. The default value of listAutoPadding is 40, which gives lists a default indent.

The padding properties do not, by default, inherit. The “auto” and “inherit” values are constants defined by the FormatValue class.

Padding properties can be negative values.

More Help topics

[Padding changes in TLF 2.0](http://blogs.adobe.com/tlf/2010/11/padding-changes-in-tlf-2-0.html)