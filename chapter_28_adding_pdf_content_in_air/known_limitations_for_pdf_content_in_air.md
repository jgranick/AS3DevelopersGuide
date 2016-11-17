## Known limitations for PDF content in AIR {#known-limitations-for-pdf-content-in-air}

Adobe AIR 1.0 and later

PDF content in Adobe AIR has the following limitations:

• PDF content does not display in a window (a NativeWindow object) that is transparent (where the transparent

property is set to true).

• The display order of a PDF file operates differently than other display objects in an AIR application. Although PDF content clips correctly according to HTML display order, it will always sit on top of content in the AIR application&#039;s display order.

• If certain visual properties of an HTMLLoader object that contains a PDF document are changed, the PDF document will become invisible. These properties include the filters, alpha, rotation, and scaling properties. Changing these properties renders the PDF content invisible until the properties are reset. The PDF content is also invisible if you change these properties of display object containers that contain the HTMLLoader object.

• PDF content is visible only when the scaleMode property of the Stage object of the NativeWindow object containing the PDF content is set to StageScaleMode.NO_SCALE. When it is set to any other value, the PDF content is not visible.

• Clicking links to content within the PDF file update the scroll position of the PDF content. Clicking links to content outside the PDF file redirect the HTMLLoader object that contains the PDF (even if the target of a link is a new window).

• PDF commenting workflows do not function in AIR.