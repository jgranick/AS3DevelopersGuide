# Chapter 28: Adding PDF content in AIR {#chapter-28-adding-pdf-content-in-air}

Adobe AIR 1.0 and later

Applications running in Adobe® AIR® can render not only SWF and HTML content, but also PDF content. AIR applications render PDF content using the HTMLLoader class, the WebKit engine, and the Adobe® Reader® browser plug-in. In an AIR application, PDF content can either stretch across the full height and width of your application or alternatively as a portion of the interface. The Adobe Reader browser plug-in controls display of PDF files in an AIR application. modifications to the Reader toolbar interface (such as controls for position, anchoring, and visibility) persist in subsequent viewing of PDF files in both AIR applications and the browser.

**_Important:_ **_To render PDF content in AIR, the user must have Adobe Reader or Adobe® Acrobat® version 8.1 or higher installed._

**Detecting PDF Capability**

Adobe AIR 1.0 and later

If the user does not have Adobe Reader or Adobe Acrobat 8.1 or higher, PDF content is not displayed in an AIR application. To detect if a user can render PDF content, first check the HTMLLoader.pdfCapability property. This property is set to one of the following constants of the HTMLPDFCapability class:

| **Constant** | **Description** |
| --- | --- |
| HTMLPDFCapability.STATUS_OK | A sufficient version (8.1 or greater) of Adobe Reader is detected and PDF content can be loaded into an HTMLLoader object. |
| HTMLPDFCapability.ERROR_INSTALLED_READER_NOT_FOUND | No version of Adobe Reader is detected. An HTMLLoader object cannot display PDF content. |
| HTMLPDFCapability.ERROR_INSTALLED_READER_TOO_OLD | Adobe Reader has been detected, but the version is too old. An HTMLLoader object cannot display PDF content. |
| HTMLPDFCapability.ERROR_PREFERRED_READER_TOO_OLD | A sufficient version (8.1 or later) of Adobe Reader is detected, but the version of Adobe Reader that is set up to handle PDF content is older than Reader 8.1\. An HTMLLoader object cannot display PDF content. |

On Windows, if Adobe Acrobat or Adobe Reader version 7.x or above is running on the user&#039;s system, that version is used even if a later version that supports loading PDF is installed. In this case, if the value of the pdfCapability property is HTMLPDFCapability.STATUS_OK, when an AIR application attempts to load PDF content, the older version of Acrobat or Reader displays an alert (and no exception is thrown in the AIR application). If this is a possible situation for your end users, consider providing them with instructions to close Acrobat while running your application. You may want to display these instructions if the PDF content does not load within an acceptable time frame.

On Linux, AIR looks for Adobe Reader in the PATH exported by the user (if it contains the acroread command) and in the /opt/Adobe/Reader directory.

The following code detects whether a user can display PDF content in an AIR application. If the user cannot display PDF, the code traces the error code that corresponds to the HTMLPDFCapability error object:

if(HTMLLoader.pdfCapability == HTMLPDFCapability.STATUS_OK)

{

trace(&quot;PDF content can be displayed&quot;);

}

else

{

trace(&quot;PDF cannot be displayed. Error code:&quot;, HTMLLoader.pdfCapability);

}