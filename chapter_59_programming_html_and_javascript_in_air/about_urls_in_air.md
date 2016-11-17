## About URLs in AIR {#about-urls-in-air}

Adobe AIR 1.0 and later

In HTML content running in AIR, you can use any of the following URL schemes in defining src attributes for img, frame, iframe, and script tags, in the href attribute of a link tag, or anywhere else you can provide a URL.

| **URL scheme** | **Description** | **Example** |
| --- | --- | --- |
| file | A path relative to the root of the file system. | file:///c:/AIR Test/test.txt |
| app | A path relative to the root directory of the installed application. | app:/images |
| app-storage | A path relative to the application store directory. For each installed application, AIR defines a unique application store directory, which is a useful place to store data specific to that application. | app-storage:/settings/prefs.xml |
| http | A standard HTTP request. | [http://www.adobe.com](http://www.adobe.com/) |
| https | A standard HTTPS request. | https://secure.example.com |

For more information about using URL schemes in AIR, see

“URI schemes” on page 813

.

Many of AIR APIs, including the File, Loader, URLStream, and Sound classes, use a URLRequest object rather than a string containing the URL. The URLRequest object itself is initialized with a string, which can use any of the same url schemes. For example, the following statement creates a URLRequest object that can be used to request the Adobe home page:

var urlReq = new [air.URLRequest(&quot;http://www.adobe.com/&quot;);](http://www.adobe.com/)

For information about URLRequest objects see

“HTTP communications” on page 811

.